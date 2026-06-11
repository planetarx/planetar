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
       │                   zbroker0 BUS (single host)                 │
       │   SHM ring (16 MB, memfd)   ·   TCP bridge   ·   UDP bridge │
       │   CAS reserves · WAL (CRC32, 64MB segments) · cross-xport   │
       │                                                              │
       │     Measured p50=80–140 ns · p99=400–900 ns (SHM, 2026-04-27)│
       └───────┬──────────────┬─────────────┬──────────┬─────────────┘
               ▼              ▼             ▼          ▼
       ┌─────────────┐  ┌────────────┐ ┌──────────┐ ┌────────────┐
       │  DETECTORS  │  │   ENTITY   │ │  WAL /   │ │  VIEWERS   │
       │ ais.gap     │  │   GRAPH    │ │  REPLAY  │ │ map        │
       │ sar.chip    │  │  (patent-  │ │          │ │ timeline   │
       │ eo.chip     │  │   backed)  │ │  replay  │ │ entity-card│
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

## Layer 2 — The bus (`zbroker0` / `broker-unified`)

### Data plane

- **SHM ring buffer** is the fastest path. A 16 MB region is created by `broker-unified` via `memfd_create`, with a cache-aligned 64-byte header holding the `write_idx`. The memfd and an eventfd are handed to producers and consumers over a Unix `SOCK_SEQPACKET` with `SCM_RIGHTS` — no filesystem path, no permissions issue, no mmap-of-file.
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

- **TCP** (11001 producer, 11002 consumer) — 4-byte length prefix, epoll. ~100–200 µs p50.
- **UDP** (11003) — raw datagrams, `SUBSCRIBE\n` to register. ~100 µs p50.
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

Detectors planned for the 1a demo (scope option A, per user):

| Topic out | Role | Implementation path |
|---|---|---|
| `ais.v1.Gap` | AIS transponder silence past expected interval for a known vessel | Heuristic on AIS stream; small, shippable |
| `sar.v1.Detection` | Ship chip from Sentinel-1 | Public S1 ship-detection baseline (xView3 or open port of CFAR + CNN chip classifier) |
| `eo.v1.Detection` | Ship chip from surface EO | Fine-tuned detector on Singapore Maritime Dataset / MODS |
| `acoustic.v1.Event` | Vessel transit from hydrophone | ORCA-SLANG-style semi-supervised classifier, applicant's own prior methodology |
| `vessel.v1.ReIDCandidate` | Cross-modal re-ID from gap + concurrent detections | The 1a's own research contribution; fusion lives here |

Detectors can be Python processes bound to protobuf-c over Unix socket (fine for SAR/EO/acoustic — they run at image/frame rate, well inside bus budget) or C processes reading SHM directly (only needed if throughput demands it).

---

## Layer 4 — Entity graph (grounded in US Patent 10,936,582 and the `doibio` POC)

This layer is **not new design work** for the 1a. The applicant is named inventor on a granted patent covering the architecture (US 10,936,582, *Integrated entity view across distributed systems*, 2021) and a working POC implementation in `~/data/dev/doibio` that has been hardened through ~18 months of iteration. The 1a productionizes that design and binds it to the planetar bus, with vessels as the entity type instead of researchers/people.

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

doibio is event-sourced: an append-only log (`vault/_logs/*.md`) is the source of truth, SQLite is a queryable projection, markdown files are a human-readable projection, git provides version control. **All mutations flow through `POST /api/events`; nothing mutates the projections directly.** This is the same pattern planetar uses — except planetar's "event log" is the zbroker0 WAL, and the projections are the entity graph + the viewer state + (optionally) a relational store for dashboards.

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

### Why the patent claim is now strong

The proposal does not say "we have a granted patent for this." The proposal says: *"The applicant is named inventor on US Patent 10,936,582 covering the integrated-entity-view architecture; that architecture is implemented in the doibio POC (~20 k LOC, 35 entity schemas, working identity-resolution engine, working event-sourced backend, ~18 months of iteration); planetar productionizes the implementation, retypes it for vessels, and binds it to the nanosecond bus."* Every clause is auditable. The patent is the IP credential; doibio is the implementation evidence; planetar is the productization. Three-step argument. All three steps verifiable.

---

## Layer 5 — The shell (sales4 → planetar-ui)

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
