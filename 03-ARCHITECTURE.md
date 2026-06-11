# 03 — Architecture

## One-page picture

```
       ┌─────────────────────────────────────────────────────────────┐
       │                       INGRESS ADAPTERS                      │
       │   AIS feed   Sentinel-1 SAR   EO cameras   Hydrophone   RF  │
       │      │             │              │            │         │  │
       │      └─────────────┴──────┬───────┴────────────┴─────────┘  │
       │                           ▼                                 │
       │              zmesg envelope (UUIDv7, ns ts,                 │
       │              topic, correlation_id, causation_id)            │
       └─────────────────────────────┬───────────────────────────────┘
                                     ▼
       ┌─────────────────────────────────────────────────────────────┐
       │               planetar-broker BUS (single host)              │
       │   SHM ring (16 MB, memfd)   ·   TCP bridge   ·   UDP bridge │
       │   CAS reserves · WAL (CRC32, 64MB segments) · cross-xport   │
       │                                                              │
       │     Predecessor zbroker0 SHM p50=80–140 ns · p99=400–900 ns │
       │              (2026-04-27); planetar-broker re-benchmark WIP │
       └───────┬──────────────┬─────────────┬──────────┬─────────────┘
               ▼              ▼             ▼          ▼
       ┌─────────────┐  ┌────────────┐ ┌──────────┐ ┌────────────┐
       │  DETECTORS  │  │   ENTITY   │ │  WAL /   │ │  VIEWERS   │
       │ ais.gap     │  │   GRAPH    │ │  REPLAY  │ │ map        │
       │ sar.chip    │  │            │ │          │ │ timeline   │
       │ eo.chip     │  │            │ │  replay  │ │ entity-card│
       │ acoustic.ev │  │            │ │  +       │ │ waveform   │
       │ rf.emit     │  │  re-ID,    │ │  audit   │ │ channel    │
       │             │  │  prove-    │ │          │ │ (Slack-    │
       │             │  │  nance     │ │          │ │  like)     │
       └─────────────┘  └────────────┘ └──────────┘ └────────────┘
                              │                          │
                              └──────── write-back ──────┘
                                  (graph updates are
                                   themselves bus messages)
```

Everything in the diagram is a bus participant. Nothing talks to anything else except through the bus. That constraint is what delivers provenance, replayability, and swap-ability.

**The centerpiece (1a R&D).** Between the detectors and the entity graph sits the **learned cross-modal fusion model** (its own section below). That model is the scientific contribution of the 1a; everything else in the diagram — bus, envelope, detectors, entity graph, shell — is the substrate that feeds it and surfaces its outputs. AIS, SAR, EO, acoustic, **RF, and text** are the six ingress modalities (CH13's own *sensor / text / RF* set).

---

## Layer 1 — The envelope (`zmesg`)

Wire format (fixed binary header, `zmesg.h`):

| Offset | Size | Field |
|---|---|---|
| 0 | 4 | magic `"ZMSG"` |
| 4 | 1 | version (1) |
| 5 | 1 | flags |
| 6 | 2 | header_len |
| 8 | 16 | id (UUIDv7) |
| 24 | 8 | `created_at_ns` |
| 32 | 8 | `stored_at_ns` |
| 40 | 8 | `published_at_ns` |
| 48+ | variable | topic, source, schema_name, correlation_id, causation_id |
| ... | variable | payload (opaque to the broker) |

Properties the proposal will claim and defend:

- **Zero-copy parse.** Deserialization only sets pointers into the source buffer. No allocations on the hot path.
- **Nanosecond ingress stamping.** `clock_gettime(CLOCK_REALTIME)` provides sub-microsecond resolution; the 8-byte field preserves it all the way through. Every output can be tied back in time to the exact moment it was observed.
- **Opaque payload.** The broker never parses payloads. Payloads are protobuf by convention (`proto/envelope.proto`) but the bus is payload-agnostic — critical for adding new modalities without touching the spine.
- **Correlation / causation ids** are mandatory for any derived message. This is what makes the entity graph explainable instead of merely provenance-tagged.

---

## Layer 2 — The bus (`planetar-broker`)

### Data plane

- **SHM ring buffer** is the fastest path. A 16 MB region is created by `planetar-broker` via `memfd_create`, with a cache-aligned 64-byte header holding the `write_idx`. The memfd and an eventfd are handed to producers and consumers over a Unix socket with `SCM_RIGHTS` — no filesystem path, no permissions issue, no mmap-of-file.
- **Lock-free CAS reserves.** Producers `fetch_add` the `write_idx` to claim a slot; they then write the entry; then `__atomic_store` a final "committed" marker. Consumers busy-poll or wait on the eventfd.
- **No mutex on the data path.** The segment-rotation mutex is on a different cache line from `write_idx` and is only taken on the rare 64 MB boundary crossing.

### Measured performance

Initial reproduction (2026-04-24, no tuning):

| Run | Messages | p50 | p99 | Avg | Min | Max |
|---|---|---|---|---|---|---|
| 1 | 10k | 140 ns | 480 ns | 160 ns | 50 ns | 8.2 µs |
| 2 | 100k | **80 ns** | 350 ns | **100 ns** | 60 ns | 20.6 µs |

Formal benchmark (2026-04-27, 1M-message scale, full detail in `docs/benchmark-2026-04-27.md`):

| Run | Messages | Config | p50 | p99 | Avg | Throughput |
|---|---|---|---|---|---|---|
| 1 | 1M | default scheduling | **80 ns** | 860 ns | 660 ns | 1.80 M msg/s |
| 2 | 1M | `taskset` core-pinned | **80 ns** | **520 ns** | 420 ns | 1.90 M msg/s |
| 3 | 100k | 1 µs producer interval | 130 ns | 420 ns | 1.57 µs | 18.7 k msg/s |

No `isolcpus`, no `SCHED_FIFO`, no huge pages, no kernel bypass. The proposal headline (`p50 = 80–140 ns, p99 = 400–900 ns`) bracketing the four-run range is the conservative claim; it will not advertise post-tuning numbers we have not measured.

### Control plane

- Unix socket `/tmp/broker-ultra.sock` handles connect / subscribe / the SCM_RIGHTS fd handoff.
- Admin HTTP (:7001 in broker-v2 variant) exposes ring depth, subscriber count, WAL state — trivial prometheus scrape.

### Transports beyond SHM

- **TCP** (12001 producer, 12002 consumer) — 4-byte big-endian length prefix, epoll, server-side topic filter via `SUB <topic>[ <topic>]*\n` handshake; `**` wildcard matches every envelope. ~100–200 µs p50.
- **UDP** (12003) — one zmesg envelope per datagram. ~100 µs p50.
- **Cross-transport fan-out.** A TCP producer's message reaches SHM consumers and vice-versa, through the shm_bridge thread. This matters for edge deployment where some sensors are remote over TCP and some compute is colocated over SHM.

### Durability

- **WAL** (`wal/segment-NNNNNNNNNN.wal`) — 64 MB mmap'd segments, magic-tagged entries, CRC32 per entry, auto-rotation.
- **Recovery.** On restart the broker scans the last segment for the valid write tail and resumes the sequence number there.
- **Per-consumer cursors** in `wal/offsets.dat` let subscribers restart and resume without re-reading the ring.
- **Replay / stream.** `./writer-wal replay` prints entries; `./writer-wal stream` re-publishes them into the ring. This is the basis of any "audit" or "what-did-the-system-see-at-18:34:22.118421036" analyst action.

---

## Layer 3 — Detectors (one consumer per modality)

Each detector is a process that:
1. Subscribes to one or more input topics (`ais.v1.Position`, `sar.v1.Tile`, `acoustic.v1.Frame`, …).
2. Runs its model / heuristic.
3. Publishes a derived topic (`ais.v1.Gap`, `sar.v1.Detection`, `acoustic.v1.Event`, …) with `causation_id` pointing back at the input envelopes.

Detectors planned for the 1a demo (scope option A, per user). **Four of five ship at project start as broker-integrated services** (see `docs/built-services-inventory.md` for LOC + tests + last commits); the cross-modal fusion detector is the 1a's new R&D:

| Topic out | Role | Implementation path | Status at project start |
|---|---|---|---|
| `ais.v1.Gap` | AIS transponder silence past expected interval for a known vessel | Heuristic on AIS stream (`planetar-ais` Node service, live Victoria BBox) | **Working — ingress shipped; gap-detector finalization is M3** |
| `sar.v1.Detection` (`sar.chip`) | Ship chip from Sentinel-1 | `planetar-sat` Python service (1,583 LOC, 5 tests): Sentinel-1 GRD fetch → CFAR + land-mask → IoU tracker → typed bus envelopes; validated on 433 Mpx scene | **Working prototype, broker-integrated** |
| `eo.v1.Detection` | Ship chip from surface EO | `planetar-eo` Python service (1,786 LOC): webcam feed → YOLO11n vessel detection → typed bus envelopes; Victoria POC sources wired (CHEK / BC Ferries / ONC / Hakai) | **Working prototype, broker-integrated** |
| `acoustic.v1.Event` | Vessel transit from hydrophone | `planetar-acoustic` Python service (2,621 LOC, 5 tests): hydrophone source → CAR-FAC + Lyons SAI → CV classifier → typed bus envelopes | **Working prototype, broker-integrated** |
| `rf.v1.Emission` | RF emission signature | `planetar-rf` (new) — best-effort public/synthetic RF → emission-signature encoder | **M1–M2 R&D (real encoder; data-sourcing risk, see PRC-4)** |
| `text.v1.Report` | Maritime report mention (NoM / PSC / OSINT) | `planetar-text` (new) — BERT-class encoder → typed envelopes linked by name/IMO/callsign | **M1–M2 R&D (public text)** |
| `vessel.v1.ReIDCandidate` | Cross-modal re-ID — the learned fusion model's output | The 1a's research contribution; see "The learned cross-modal fusion model" below | **M3 R&D — the centerpiece** |

Detectors are Python processes that emit zmesg envelopes over TCP to `planetar-broker` port 12001 (4-byte big-endian length prefix + zmesg). They run at image/frame rate, well inside bus budget. M3 work converts these prototypes into production-grade typed-bus participants with full causation lineage and adds the `vessel.ReIDCandidate` fusion consumer on top.

### MediaPipe as the edge-perception runtime (SWaP profile + agentic graph adaptation)

The detectors above run today as Python processes (YOLO11n for EO, CAR-FAC + CV for acoustic, CFAR for SAR). For the **edge / SWaP profile** — CH13's *Edge Fusion for Tactical Units* example (audio + video + sensor on wearables under degraded connectivity) and Desired Outcome #5 — the **proposed** perception stage targets **Google MediaPipe** [H1], the text-defined (`.pbtxt`) calculator-graph runtime built for real-time **on-device** vision / audio / sensor inference. This is a 1a proposal, not a current dependency: today's detectors are the Python services above; MediaPipe is the proposed edge runtime, drawing on the applicant's few years of hands-on experimentation with the framework (`04-PORTFOLIO.md` J).

Two properties make MediaPipe load-bearing rather than cosmetic:

- **Text-defined graphs → runtime-mutable perception.** A MediaPipe pipeline is a `.pbtxt` graph of calculators; swapping a model or a whole subgraph is a text edit, not a recompile. The perception pipeline is therefore an *artifact the system itself can rewrite*.
- **Agentic graph adaptation (the 1a's edge R&D).** An agentic controller rewrites the MediaPipe graph to optimize classification under changing constraints — a lighter network under power/thermal limits, a heavier classifier under high-threat conditions, a different modality mix when a sensor degrades. This realizes the CFP's stated goal of *"learned, adaptive fusion across modalities rather than static aggregation"* and *"constraint-aware AI models"* — and every graph rewrite is itself a provenance event on the bus (`detector.v1.GraphRewrite`, `causation_id` → the condition that triggered it), so the adaptation is auditable like everything else. **TRL 1–2 at start; genuine new R&D, not plumbing.** *[Proposed, not prototyped — confirmed by applicant 2026-05-30.]*

---

## The learned cross-modal fusion model (1a R&D — the centerpiece)

The scientific contribution of the 1a. The detectors (Layer 3) produce per-modality observations; the fusion model learns to associate them into vessel identities — even when AIS is dark.

**Architecture (proposed, TRL 2 → 3).**

1. **Per-modality encoders** project each observation into a shared embedding: SAR-chip CNN, EO-chip CNN, acoustic spectrogram / CAR-FAC → CNN, AIS kinematic + text-field sequence encoder, RF emission-signature encoder, and a BERT-class text encoder [I7] for maritime reports [I1, I2].
2. **Self-supervision from AIS co-occurrence — the key idea.** During AIS-on periods a vessel's identity labels the concurrent observations in its space-time neighbourhood *for free*; the model learns the cross-modal association (contrastive / joint-embedding, I-JEPA family [I6]). No manual labels required.
3. **Association head.** A transformer over the set of concurrent observations [I3] groups them into identities (set → identity), analogous to metric-learning re-ID [I4].
4. **Uncertainty.** Per-modality evidential heads [I5] propagated through the fusion; conformal calibration [E1, E2] on the fused score for distribution-free coverage.
5. **Inference on dark vessels.** With AIS absent, the learned association re-identifies the vessel from the remaining modalities, emitting `vessel.v1.ReIDCandidate` with calibrated confidence and attention-weighted evidence.
6. **Explanation + human-in-the-loop.** Attention weights + the causation chain are the operator's explanation; analyst adjudication corrects the graph and feeds an active-learning loop (Layer 5).

**Feasibility.** The encoders reuse the four shipped detectors; AIS supplies free supervision; the model fine-tunes public baselines rather than training at frontier scale. The applicant's semi-supervised deep learning on unlabelled acoustic archives [A1, A2] is the direct precedent. **Honest TRL: 2 → 3 over the 1a.** The output, `vessel.v1.ReIDCandidate`, is a bus envelope whose `causation_id` references its inputs — so the model's inferences inherit the substrate's provenance and replay for free.

---

## Layer 4 — Entity graph (`planetar-ontology`, developed beyond US Patent 10,936,582 and the `doibio` POC)

This layer is **not new design work** for the 1a. The applicant is a named inventor (Salesforce-assigned) on a granted patent covering the integrated-entity-view architecture (US 10,936,582, *Integrated entity view across distributed systems*, 2021) and has a working POC implementation in `~/data/dev/doibio`, hardened through ~18 months of iteration. The 1a's entity graph is a substantial development beyond that background — not a re-implementation of it.

**Production successor shipping at project start: `planetar-ontology`** — TypeScript / Node, zero npm dependencies (native `node:sqlite`), 2,323 LOC, **30 tests passing**, broker-integrated as a TCP subscriber on port 12002. Phases P1–P5 are complete:

- **P1** — zmesg codec + schema registry (envelopes parsed and classified by `schema_name`).
- **P2** — identity resolution + merge (lifts the `doibio` confidence-weighted algorithm).
- **P3** — Object API (`GET /schema`, `GET /objects/...`, `WS /subscribe` — read paths the shell consumes).
- **P4** — Action executor (write-back via bus messages).
- **P5** — kinematic match rules incl. **dark-vessel re-ID** (the load-bearing CH13 rule).

The 1a's research at this layer (M4) is the **vessel-domain Party-model retype** layered onto this scaffold and **conformal-prediction calibration** of the fused score — not the scaffolding itself, which is done. The `doibio` POC remains the pattern-donor and the 18-month-iteration evidence; `planetar-ontology` is what the proposal stands on.

### The Party model — what gets lifted from doibio

doibio implements a Salesforce-inspired *Party model* that cleanly separates the canonical entity from its observed identities and from its observed attributes. Mapped to vessels:

| doibio type | planetar vessel analog | Role |
|---|---|---|
| **Party** (`pty_…`) | **Vessel** (`ves_…`) | Canonical entity. The thing the analyst tracks. |
| **PartyIdentification** (`pid_…`) | **VesselIdentification** | Observed identifiers: MMSI, IMO, callsign, name-as-painted, hull number, transponder fingerprint, RF-emission signature, acoustic signature. Each PID points to one Vessel; many PIDs may share the same Vessel. |
| **Individual** / **Organization** | **VesselDetails** / **Operator** | Demographic/structural data: vessel class, length, flag state, operator entity. |
| **PartySource** | **ObservationSource** | Provenance: where this fact came from, when, by what method, with what data-quality score. **Every fact has a source record.** |

This structure is what makes "the same vessel under different names" tractable. AIS-as-MMSI, SAR-chip-as-image-fingerprint, hydrophone-as-acoustic-signature, EO-as-visual-fingerprint are all **PartyIdentifications pointing at the same Party**, with confidence weights on the linkages.

### The identity-resolution algorithm — already implemented

doibio's `src/lib/identity-resolution.ts` (630 lines, production-grade) implements a confidence-weighted matching engine that planetar inherits as a starting point. Match types and weights, mapped from doibio's domain (people) to planetar's (vessels):

| Match type | doibio (people) | planetar (vessels) | Confidence weight |
|---|---|---|---|
| Exact ID | ORCID / GoogleScholar | MMSI / IMO / callsign | 1.00 |
| Strong identifier | email | RF-emission fingerprint | 0.90 |
| Fuzzy name (Levenshtein) | name | name-as-painted, OCR'd | 0.70 |
| Institutional | organization | flag state + operator | 0.50 |

Suggestion thresholds (also lifted): **merge ≥0.95**, **link ≥0.80**, **review ≥0.70**, **new <0.70**. The thresholds are tunable per-deployment and the model is calibrated rather than hand-set — the user already has experience tuning these in doibio.

The 1a's research contribution at this layer is the **acoustic + SAR fingerprint matchers** that don't yet exist in doibio's people-centric implementation: how to score "is the vessel I see in this Sentinel-1 chip the same one I heard pass by station VENUS 41 minutes ago" with calibrated confidence.

### Event sourcing as the single source of truth

doibio is event-sourced: an append-only log (`vault/_logs/*.md`) is the source of truth, SQLite is a queryable projection, markdown files are a human-readable projection, git provides version control. **All mutations flow through `POST /api/events`; nothing mutates the projections directly.** This is the same pattern planetar uses — except planetar's "event log" is the `planetar-broker` WAL, and the projections are the entity graph + the viewer state + (optionally) a relational store for dashboards.

Properties this gives planetar for free:

- **Time-travel queries.** "What did the system know about vessel X at 18:34:22.118421036?" is a cursor-into-WAL operation, not a backup-restore exercise.
- **Audit replay.** Any output of the system can be reproduced bit-for-bit by replaying its inputs from the WAL.
- **Schema evolution.** New entity types and new observation types are additive; old events still replay because the broker doesn't parse payloads.
- **External read access.** Reviewers / auditors get a read-only consumer subscription — same model as the analyst, no new auth path.

### Confidence and adjudication — where `crank3` plugs in

doibio currently uses simple weighted thresholds. planetar extends this with the log-score decomposition from `~/github/sness23/crank3` — prior + per-modality evidence + uncertainty + penalty — for the multi-method case where SAR + EO + acoustic + RF + AIS-gap all simultaneously vote on a re-ID. This adds calibrated uncertainty to the matching engine.

### Write-back is itself a bus message

When the graph publishes `vessel.v1.ReIDCandidate`, that publication is *itself* an envelope on the bus, with `causation_id` pointing back to the input observations and `correlation_id` carrying the analyst session. The shell surfaces it, the WAL records it, and any future reviewer can reconstruct exactly what the system inferred and from what — Palantir Ontology's "provenance," Kafka's "log compaction," and Slack's "thread anchored to a message" all collapse into one mechanism.

### What planetar inherits cleanly from doibio (lift)

- **Event log format** (markdown frontmatter + JSON body + diff). Human-readable, machine-parseable, git-friendly.
- **Identity-resolution engine** (`src/lib/identity-resolution.ts`). 630 lines, working.
- **Party / PartyIdentification / PartySource schemas** — re-typed for vessels.
- **ULID + type-prefix ID system** (`pty_01HX…`, etc.). Sortable, collision-free, decodable.
- **JSON Schema validation pipeline** + git hooks. 35 schemas in `vault/_schemas/`.
- **Salesforce-Party-model + Palantir-Ontology research** — `docs/RESEARCH-palantir-ontology.md`, `docs/RESEARCH-linkedin-kafka-architecture.md` show the design choices were made deliberately, not accidentally.

### What planetar rebuilds (don't lift)

- **2-second polling** for real-time updates → replace with bus subscription (this is exactly what the bus is for).
- **Hardcoded Cohere AI integration** → pluggable model layer.
- **Stubbed merge command** → atomic merge as a typed bus event.
- **Single SQLite channels.db** → channel store on the bus, projection in the entity store.
- **Ad-hoc Elasticsearch index** → search as a first-class projection of the WAL.

### Why the entity-graph claim is now strong

The proposal does not say "we own a granted patent for this" — and it does not lean on the patent as the novelty. It says: *"The applicant is a named inventor (Salesforce-assigned) on US Patent 10,936,582 covering the integrated-entity-view architecture; that architecture is implemented in the doibio POC (~20 k LOC, 35 entity schemas, working identity-resolution engine, working event-sourced backend, ~18 months of iteration); planetar develops it substantially further — retypes it for vessels, rebuilds it zero-dep, and binds it to the nanosecond bus."* Every clause is auditable. The prior patent is named-inventor background; doibio is the implementation evidence; planetar is the new work. Each step verifiable.

---

## Layer 5 — The collaborative-intelligence layer (shell: sales4 → planetar-ui)

Base: `~/github/sness23/sales4` — React 19 + Vite + TypeScript, WebSocket server with microsecond-precision latency instrumentation, two-phase RTL-based E2E latency calculation. Already a working Slack/Discord-style multi-client app.

**Rename in proposal context:** `planetar-ui` (the repo name stays for now; the proposal won't reference "sales4").

### Shell = viewers as bus consumers

The shell is a React app with a WebSocket connection to a thin gateway (the existing sales4 server, extended). The gateway subscribes to bus topics and forwards typed messages to the browser. Each viewer is a component that subscribes to a subset of topics:

| Viewer | Subscribes to | Renders |
|---|---|---|
| **Map** | `ais.v1.Position`, `sar.v1.Detection`, `eo.v1.Detection`, `vessel.v1.ReIDCandidate` | Live vessel tracks, dark-period interpolations, detection pins, confidence halos |
| **Timeline** | all `*.v1.*` topics | An event ribbon keyed by `created_at_ns`, scrubbable, filterable by topic |
| **Entity card** | `vessel.v1.*` | One entity: its re-ID candidates, its evidence list, its graph neighborhood, clickable links to the underlying envelopes |
| **Waveform / chip** | `sar.v1.Tile`, `acoustic.v1.Frame`, `eo.v1.Frame` | Raw media viewer — SAR chip, audio spectrogram, EO crop — so the analyst can adjudicate |
| **Channel** | `chat.v1.Message`, `analyst.v1.Action` | The Slack-style conversation; the analyst's own actions are messages on the bus too |

All five viewers are **the same application**. They're arranged in a multi-panel layout (like Slack has channels + a thread pane + a details panel). Switching context doesn't reload state; the state is the bus.

### Why the shell is on the bus, not on an HTTP API

- **Replay is free.** Analyst opens "what did we see during the dark period?" — the shell subscribes with a historical cursor into the WAL and the same viewers render the past exactly as they render the present.
- **Reviewer / auditor access is free.** The same subscription model gives an external reviewer a read-only view — no separate BI tool, no ETL.
- **Collaboration is free.** Analyst A's cursor position is a `analyst.v1.CursorMoved` message; Analyst B's viewer sees it. (Proposal will not overclaim this — scope for 1a is single-analyst; multi-analyst is a follow-on.)

### The human in the loop — collaborative intelligence

CH13 asks for *"explainable outputs for operator trust"* and decisions for *"operational decision-making."* planetar treats this as a **Collaborative Intelligence (CI)** problem — the CSCW-rooted stance [G1–G7] that a human–AI system outperforms either alone when the operator is embedded *in* the loop, empowered by the AI rather than positioned downstream of it. This is not a new bolt-on: it is the applicant's shipped, peer-reviewed prior art — the Orchive [A6a][A6b][A9], a collaborative web platform where expert researchers and citizen scientists added **18,000+ annotations** to a 20,000-hour orca-call archive, training classifiers whose outputs were shown back in the same interface (the thesis frames this as *"Intelligence Augmentation"*); `04-PORTFOLIO.md` G — now expressed in the bus architecture.

Concretely, the operator is a **first-class bus participant**, not a consumer of a finished answer:

- **Adjudication is a message.** When the analyst accepts, rejects, or corrects a `vessel.v1.ReIDCandidate`, that decision is an `analyst.v1.Action` envelope with `causation_id` → the candidate and its evidence. It writes back to the entity graph (write-back is already a bus message, Layer 4) and is preserved in the WAL like any observation.
- **The decision is shared, and faster.** The AI does the high-volume cross-modal correlation; the human does the high-stakes adjudication; the shell collapses the latency between them. This is the mechanism behind CH13's *"increase speed of decision making"* — not full automation, but a tightened human+AI loop.
- **Adjudications close an active-learning loop.** Accept/reject/correct labels feed back [G5] to refine detector and fusion thresholds over time — the same human-improves-the-model dynamic the Orchive used at archive scale. (1a scope: the loop is designed and the labels are captured; online retraining is a follow-on phase — stated, not overclaimed.)
- **Trust is calibrated, not asserted.** Every output carries its causation chain (click-through to raw inputs) *and* a conformal-calibrated confidence — the analyst sees both *why* and *how-sure*, which is what converts an explainable output into a trusted decision [G4, G6].

This layer is where planetar's *"policy-aware, explainable"* requirement becomes operational: a human accountable in the loop is a **structural** property — which is also the spine of the GBA+ argument (PRC-5: designing against automation bias and for diverse operators).

---

## What we build in the 1a (option A scope, per user)

1. **Bus & envelope as-is**, re-packaged as `planetar-bus` under Zax Analytics branding. No new architecture — we're proving the one we have.
2. **One new ingress adapter per modality**, on public data:
   - AIS via MarineCadastre / AIS-Hub replay
   - SAR via Sentinel-1 Copernicus
   - EO via Singapore Maritime Dataset
   - Hydrophone via Ocean Networks Canada public feeds (or MobySound archive)
3. **Five detectors** in the table above.
4. **Entity graph** — new, shipped with the 1a. DuckDB-backed, replayable.
5. **Shell extensions** — five viewers on top of sales4 comms-app. Map and entity-card are the biggest net-new work.
6. **Demo scenario** — a synthetic "vessel goes dark in the Salish Sea during a Sentinel-1 pass while a hydrophone station records it" scenario, end-to-end, replayable.

Deliverable is a running system the reviewer can drive in a browser plus a measured architecture report.

See `07-TIMELINE.md` for the calendar breakdown.
