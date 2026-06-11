# 02 — Technical Strategy

## The pitch in one sentence

**planetar** is a **learned cross-modal fusion model** for maritime domain awareness. Six heterogeneous streams — AIS, SAR, EO, passive acoustic, RF, and textual maritime reports — are encoded into a shared spatiotemporal embedding where one vessel's observations resolve to a single calibrated, uncertainty-scored, explainable identity, **even when its AIS beacon is dark** (the 1a flagship). A working real-time, provenance-tracked substrate the applicant has already built — typed bus, entity graph, analyst shell — makes the model deployable at the edge, explainable, and replayable.

**The model is the thesis; the substrate is the enabler.** The science of the 1a is the learned fusion itself: a self-supervised model that uses AIS-on co-occurrence as a free training signal to learn cross-modal association, then re-identifies AIS-off vessels. See `docs/recenter-learned-fusion.md`.

---

## Why this is a CH13 answer, not a generic platform pitch

CH13 asks for an AI model that *learns* to fuse. planetar answers each demand:

1. **An AI model fusing ≥ 2 heterogeneous types** (essential outcome). Six — AIS, SAR, EO, acoustic, RF, text — encoded into one learned model, against the challenge's own *sensor / text / RF* example.
2. **Learned, adaptive fusion, not static aggregation.** A trained cross-modal association model, self-supervised on AIS-on co-occurrence; its perception front-end adapts to SWaP / threat at the edge.
3. **Spatiotemporal alignment + uncertainty + confidence.** Alignment is learned in the shared embedding; per-modality evidential uncertainty is propagated and conformally calibrated.
4. **Entity resolution + dynamic knowledge graph.** Resolved identities populate the vessel-entity graph — developed beyond US 10,936,582 (applicant-named inventor, Salesforce-assigned) — with full provenance.
5. **Policy-aware lineage across classification levels.** Every observation and inference is traceable to its inputs; policy filtering at access time.
6. **Explainable outputs for operator trust.** Attention over modalities + click-through causal evidence; a human analyst in the loop.
7. **SWaP / edge.** The substrate runs on commodity hardware; the perception graph is runtime-rewritable for constrained devices.

---

## The flagship application: dark-vessel detection (AIS-off)

**Problem.** When a vessel disables its AIS transponder, it disappears from the default maritime picture. This is used for illegal fishing, sanctions evasion, ship-to-ship transfers, and — in the Arctic — for sovereignty-testing incursions. CH13's *Maritime Task Group Operations* example is exactly this class of problem.

**What planetar does.**

1. **Ingest** continuous streams from each modality as typed bus messages:
   - `ais.v1.Position` — every AIS broadcast seen (and, implicitly, every gap where one should be).
   - `sar.v1.Detection` — ship chips extracted from Sentinel-1 SAR tiles.
   - `eo.v1.Detection` — ship chips from Singapore Maritime / MODS surface EO.
   - `acoustic.v1.Event` — hydrophone event detections (ORCA-SLANG-style semi-supervised archival ML).
   - `rf.v1.Emission` — non-AIS RF emissions, where public datasets permit.

2. **Detect the gap.** An `ais.gap` detector consumes `ais.v1.Position` and publishes `vessel.v1.WentDark{mmsi, last_known_position, t}` whenever a known vessel's AIS goes silent where it shouldn't.

3. **Re-identify (the learned model).** The cross-modal fusion model embeds the concurrent SAR/EO/acoustic/RF/text observations and associates them to a vessel identity learned from AIS-on co-occurrence, publishing `vessel.v1.ReIDCandidate{entity_id, evidence, score, uncertainty}` — calibrated, with per-modality attention as its explanation.

4. **Surface.** Viewers subscribe:
   - **Map viewer** renders tracks and dark-period interpolations.
   - **Timeline viewer** shows the event ribbon and lets the analyst scrub.
   - **Entity-card viewer** shows the re-ID candidates, their evidence, and the confidence + provenance chain.
   - **Waveform viewer** shows raw SAR/EO/acoustic clips for the analyst to adjudicate.
   - **Channel viewer** (the Slack-style one) is where the analyst and any adjudication bots talk about the candidate.

Everything the analyst sees is a projection of the same bus. Reproducibility is not a feature; it's the ground truth.

---

## Architectural claims that are defensible today

| Claim | Evidence |
|---|---|
| Sub-200 ns p50 end-to-end on SHM ring, protobuf envelope, single host | Reproduced 2026-04-27 on commodity Linux: p50=80–140 ns, p99=400–900 ns over 1M-message benchmarks (`docs/benchmark-2026-04-27.md`) |
| Sub-50 µs p50 TCP path on the same bus | planetar-broker 2026-05-14: p50=34 µs, p99=424 µs at paced ~15 k msg/s, 200k 79-byte messages, i9-9900K kernel 6.17 (`docs/benchmark-2026-04-27.md` TCP addendum); raw artifacts preserved |
| Lock-free, zero-syscall, zero-copy on the hot path | planetar-broker (predecessor zbroker0) — SHM via memfd + SCM_RIGHTS, CAS reserves, eventfd only on wake |
| Durable, recoverable, cross-transport fan-out | WAL with CRC32 + segment rotation; TCP/UDP/SHM all reach all consumers |
| Nanosecond-precision ingress timestamps on every envelope | zmesg.h — `created_at_ns`, `stored_at_ns`, `published_at_ns` as uint64 ns |
| Working Slack-style multi-client shell with µs-precision latency instrumentation | planetar-ui (React 19 / TypeScript / WebSocket bridge to the broker), already microsecond-logged; predecessor sales4 comms-app + server |
| Named-inventor (Salesforce-assigned) credit on two granted patents in the entity-resolution / identity-matching space | US 10,936,582 (2021), US 11,442,952 (2022) |
| Working POC implementation of that entity architecture | doibio: 20k+ LOC, 35 entity schemas, 630-line identity-resolution engine, event-sourced backend, ~18 mo iteration |
| **Working production successor** of that architecture | `planetar-ontology` — zero-dep TS, 2,323 LOC, **30 tests pass**, broker-subscriber on port 12002, **P1–P5 shipped** incl. dark-vessel kinematic re-ID. The prior patents + doibio give the background and the design; planetar-ontology is the shipping implementation, and the 1a's learned fusion model is the new, separately patentable IP. |
| **Four broker-integrated per-modality detectors** at project start | `planetar-ais` (Node, live Victoria AIS) · `planetar-sat` (Python 1,583 LOC + 5 tests, Sentinel-1 GRD + CFAR + IoU tracker) · `planetar-eo` (Python 1,786 LOC, YOLO11n on Victoria public webcams) · `planetar-acoustic` (Python 2,621 LOC + 5 tests, CAR-FAC + Lyons SAI + CV classifier). All publish typed zmesg envelopes over TCP 12001. The 1a's R&D is the **fifth, cross-modal** detector. |
| Canonical-data-model schema codegen | `planetar-registry` — zero-dep JS, 710 LOC; SSOT for the schemas the sat/eo/acoustic detectors emit against; demos `8/8 round-trip` + `5/5 fusion` pass. |
| Audit-defensible ONC institutional bond on hydrophone ML | Sattar, Driessen, Tzanetakis, Ness, Page (IEEE PacRim 2011), 95% accuracy on NEPTUNE Canada/ONC hydrophone data, explicit acknowledgement of NEPTUNE/CANARIE support |

All the above exist as code, IP, or peer-reviewed publication **before** the 1a begins. The 1a research question is not "can this be built" — it is "can the cross-modal dark-vessel detection head be wired onto this spine and produce calibrated, explainable outputs on public data in 6 months." That's a well-scoped TRL 2 → 3 advance.

---

## What's actually built today

As of 2026-05-21 (proposal-submission month), the canonical-spine `planetar-*` stack is real, broker-integrated, open-source code — **~13 k LOC total across 9 repos, all gitleaks-scanned clean and pushed to GitHub** (full audit map: `docs/built-services-inventory.md`).

### The full spine at project start

| Layer | Service | Stack | Status |
|---|---|---|---|
| **L2 — Bus** | `planetar-broker` | C, ~1.2 k LOC (+ 190-LOC `shm-consumer`) | SHM p50 = 80–140 ns / p99 = 400–900 ns (predecessor `zbroker0`, 1M-msg 2026-04-27). TCP path p50 = 34 µs / p99 = 424 µs (planetar-broker, paced 200 k 2026-05-14). |
| **L1 — Envelope** | `zmesg` | C header-only, 260 LOC | UUIDv7, ns timestamps, full provenance. 4-byte BE length prefix on TCP/UDP. |
| **L3 — Ingress: AIS** | `planetar-ais` | Node | Live Victoria BBox AIS feed → per-MMSI bus channels. |
| **L3 — Ingress + detector: SAR** | `planetar-sat` | Python, 1,583 LOC + 5 tests | Sentinel-1 GRD fetch → CFAR + land-mask → IoU tracker → typed bus envelopes. Validated on 433 Mpx real scene. |
| **L3 — Ingress + detector: EO** | `planetar-eo` | Python, 1,786 LOC | Public webcams (CHEK / BC Ferries / ONC / Hakai) → YOLO11n vessel detection → typed bus envelopes. |
| **L3 — Ingress + detector: Acoustic** | `planetar-acoustic` | Python, 2,621 LOC + 5 tests | Hydrophone source (synth / archive / ONC / OrcaSound) → CAR-FAC + Lyons SAI → CV classifier → typed bus envelopes. |
| **L4 — Entity graph** | `planetar-ontology` | TS, 2,323 LOC, zero-dep, 30 tests pass | Bus subscriber (port 12002); P1–P5 shipped: codec + registry, identity resolution + merge, Object API, Action executor, kinematic match incl. **dark-vessel re-ID**. |
| **L4 — Canonical data model** | `planetar-registry` | JS, 710 LOC, zero-dep | Codegen SSOT: registry JSON → schemas + DDL + TS interfaces. `sat`/`eo`/`acoustic` emit envelopes conforming to schemas generated here. |
| **L5 — Shell** | `planetar-ui` | TS / React 19 / Vite | Slack/Discord/Quip/Palantir 4-pane shell, WS bridge to the broker, µs-instrumented. Predecessor `sales4`. |

**The 1a's R&D (TRL 2 → 3 over 6 months)** is the **learned cross-modal fusion model** — per-modality encoders → shared embedding → self-supervised association head (AIS-on co-occurrence as the signal) → propagated uncertainty + conformal calibration → re-identification of AIS-off vessels — plus the new **text** and **RF** encoders. The substrate is done; the research is the model.

### The doibio POC — the entity-architecture evidence

The entity-architecture claim is materially stronger because the architecture was implemented and iterated *before* `planetar-ontology` was written. `~/data/dev/doibio` is an event-sourced filesystem-first entity store the applicant has run for ~18 months. Not a sketch — production-grade for the applicant's own use. **`planetar-ontology` is the production successor** that lifts doibio's design and rebuilds it zero-dep onto the bus.

What doibio has shipped (and what `planetar-ontology` inherited):

- **Salesforce-inspired Party model.** Canonical Party (`pty_…`) ↔ many PartyIdentifications (`pid_…`) ↔ Individual / Organization details, with PartySource records tracking origin + import method + quality score. 35 JSON schemas in `vault/_schemas/`. *Inherited by `planetar-ontology` as the entity scaffold.*
- **Identity-resolution engine.** `src/lib/identity-resolution.ts`, 630 lines, production-grade. Levenshtein-based fuzzy matching + exact-ID matching; confidence scoring (1.00 exact ID / 0.90 strong identifier / 0.70 fuzzy name / 0.50 institutional); thresholds (merge ≥0.95, link ≥0.80, review ≥0.70, new <0.70). The thresholds are tuned — applicant has run them on real data. *Lifted into `planetar-ontology` P2.*
- **Event-sourced backend.** Append-only event log (`vault/_logs/*.md`) is source of truth; SQLite + markdown projections; replay and time-travel queries are first-class. The pattern matches Kafka log compaction; doibio's `docs/RESEARCH-linkedin-kafka-architecture.md` shows the design choice was deliberate, not accidental. *In planetar, the broker WAL is the event log and `planetar-ontology` SQLite is the projection.*
- **Slack-channel UI** (`comms-app/`) wired to entity store via REST + WebSocket, channels per topic, real-time updates via WS broadcast. *Reborn as `planetar-ui` (React 19, WS-to-bus bridge, predecessor was `sales4`).*
- **Schema-first validation** with AJV + git hooks (Node + Python). 35 schemas under version control. *Reborn as `planetar-registry` (codegen SSOT for schemas the detectors emit against).*
- **Explicit Palantir-Ontology and LinkedIn-Kafka research docs** in `docs/RESEARCH-*.md`. Architecture choices were made with eyes open.

What doibio's POC scaffolding got retired when `planetar-ontology` was built:

- 2-second polling for real-time updates → bus subscription on port 12002 (this is exactly what planetar-broker fixes).
- Hardcoded Cohere AI integration → pluggable model layer.
- Stubbed merge command → atomic merge as a typed bus event (P2).
- Single SQLite `channels.db` → channels as bus topics; `planetar-ontology` SQLite is now a projection of the bus, not the source of truth.

**The implication for the proposal narrative.** PRC-2 (novelty) does not say "we have an idea," nor does it lean on the prior patents as the novelty. It says: *"The entity-resolution architecture — which the applicant contributed to as a named inventor on prior Salesforce-assigned patents — is already implemented twice: in the 18-month-iterated `doibio` POC (~20 k LOC, 35 schemas, 630-line identity-resolution engine) and in the shipping production successor `planetar-ontology` (2,323 LOC zero-dep TS, 30 tests pass, P1–P5 incl. dark-vessel kinematic re-ID, broker-integrated). The 1a's genuine novelty is the learned cross-modal fusion model built on top — novel, separately patentable foreground IP."* The prior patents are background; the implementation is the evidence; the fusion model is the new work. Each clause independently verifiable.

---

## Honesty principles (inherited from v1)

1. **Third-party code is framed as power-user work, not authorship.** Aeron, Protenix, chai-lab, boltz, chemeleon, ChimeraX are other people's code. planetar's bus, envelope, and shell are the applicant's.
2. **The fusion model is proposed, not built.** It is the 1a's TRL 2→3 R&D — adapted from public architectures (CLIP / I-JEPA / set-transformer families [I2, I3, I6]) with attribution, self-supervised on AIS co-occurrence. Feasibility rests on the built substrate + the applicant's semi-supervised-DL track record, not on claiming the model already exists.
3. **TRL honesty.** Bus + envelope + shell + entity graph are TRL 3–4 today. Cross-modal dark-vessel fusion is TRL 2 today. The 1a advances the fusion component to TRL 3 on the existing spine. Framing in MC-1 matches this precisely.
4. **No marketing adjectives** in the narrative. "Nanosecond" is a measurement, not a slogan. "Palantir-like" and "Slack-style" are used to anchor reviewer intuition — the proposal will cite them once and move on to specific architectural claims.
5. **All novelty claims must survive a post-award audit.** See 04-PORTFOLIO (TODO) for the specific citable anchors.

---

## Pivot from v1, explicitly

| Dimension | v1 (MAIA-MD) | v2 (planetar) |
|---|---|---|
| Core thesis | A single JEPA fusion model | A learned self-supervised cross-modal fusion model, on a built real-time substrate |
| TRL evidence | Adaptation from adjacent published work | Substrate + detectors built (TRL 3); model TRL 2→3; applicant's semi-supervised-DL track record |
| Novelty anchor | Architectural coupling of JEPA + provenance graph | Self-supervised AIS→dark-vessel re-ID — calibrated, explainable, adaptive |
| Demo scope | Algorithmic output report | End-to-end running system the reviewer can open in a browser |
| Explainability | Post-hoc, through provenance tags | Native — attention over modalities + click-through causal chain |
| Fit to solo 1a | Risky (JEPA-scale training) | De-risked — AIS self-supervision + fine-tuning public encoders on a built substrate, no frontier training |
| Differentiation | Yet-another-fusion-architecture | A learned dark-vessel fusion model that is calibrated, explainable, and edge-deployable |

The v2 story keeps the learned model CH13 rewards while **de-risking it** with a built substrate and AIS self-supervision — **aligned with the challenge, defensible under audit, and demonstrable in 6 months.**

---

## Moats — what CH13 is *for*

> **Internal company strategy, not for submitted CH13 narratives.** This section concerns
> what the proposal is *for* — the follow-on company — not what the proposal claims.
> The canonical, longer-form version is `MOAT-STRATEGY.md`; this section is the same
> content folded into the strategy spine. Keep them in sync.

### Thesis

The moat is a **real-time entity-resolution layer that incumbents are architecturally
locked out of** — proven by an *earned secret*, validated by a competitively-won defence
contract, and compounding through a data flywheel. **CH13 is the evidence that makes the
moat fundable.**

The follow-on to Component 1a is **a venture round**, not IDEaS 1b and not a direct DND
contract. Positioning to investors: a *defence company* (Anduril-shaped) — maritime
domain awareness as the beachhead, then other defence domains. Carry the expansion map
(maritime → land / ISR → space / RF → allied MDA) so the TAM ceiling reads as high.

### The moat stack

**Tier 1 — load-bearing.**

1. **Earned secret.** Steven worked on Salesforce's Magic Bus + Canonical Data
   Architecture for Customer 360; it was too slow to unlock what was needed; he has now
   built the real-time version. Almost nobody on earth can credibly say that. Lead every
   pitch with it.
2. **Architecture lock-out — stated precisely.** Two-front argument, different fronts:
   - **vs. Palantir → counter-positioning.** Foundry / Gotham's ontology sits on batch
     semantics. Making it real-time is a *breaking change to product and customer base*,
     not a feature add. They **won't**, not can't. Salesforce had infinite resources and
     still didn't crack Customer 360 on this — that is the answer to "why hasn't the
     incumbent done it."
   - **vs. funded startups → integration + head start.** A startup can build a fast bus
     in a quarter. They cannot, in a quarter, build bus + envelope + ontology + five
     detectors + shell *co-designed* — *and* hold a won defence contract *and* a
     real-data demo. The moat is the integrated vertical plus the CH13 lead time.

   Discipline: *"we're faster"* is not a moat. *"The incumbent is structurally
   disincentivized, and competitors cannot catch the integrated head start"* is.

**Tier 2 — built by CH13, compounding.**

3. **Ontology as system-of-record.** Once entities live in the graph with full lineage,
   switching means re-resolving history. Defence customers never rip out a
   system-of-record.
4. **Dark-vessel re-ID corpus / data flywheel.** Every confirmed re-identification is a
   labeled example; defence-grade fusion data is uniquely hard to obtain. *Caveat: at
   TRL 1–3 over one project the corpus is small — pitch it as "flywheel designed in,
   first data points collected," never "we have a data moat today."*
5. **Accreditation-readiness package** (now a real Phase-1a deliverable). In defence
   this is a **time moat** — ATO processes run for years; a started, documented one only
   you understand is a head start competitors cannot buy.

**Tier 3 — supporting.** Thin open envelope/protocol as an interoperability standard
(standard-capture at near-zero cost). Modality-onboarding speed (a demo / sales asset).

### CH13 → venture bridge

Design every CH13 deliverable to **double as a venture asset.**

| CH13 deliverable                | Venture moat asset                                       |
|---------------------------------|----------------------------------------------------------|
| Live dark-vessel demo           | The headline slide — "it works," not "it could"           |
| Ontology / canonical data model | The system-of-record asset                                |
| Re-ID corpus                    | Flywheel evidence                                         |
| Accreditation-readiness package | "Defence-deployable" credential + time moat               |
| The won award                   | Third-party validation + first-customer signal            |
| Benchmark report                | Proof the architecture claim is measured, not marketing   |

Headline proof point for the raise: the **live dark-vessel demo**.

### Raise timing

**Recommended: raise on deliverables (late 2026 / early 2027); warm investors now.**

The proof point is the live demo; raising on the award alone is raising on a promise.
Demo + corpus + accreditation package collectively de-risk the three questions a defence
VC asks (does it work / does the data compound / is it deployable). Pre-award is the
weakest moment. Investor relationships take 6–9 months to warm — start *conversations*
now, open the actual round once the demo is live.

**Flips if runway is the constraint.** If CH13's ≤$250K CAD does not cover the founder
through delivery, a small angel / pre-seed bridge may be needed sooner.
*Open: does CH13 funding cover runway through delivery, or is there a gap?*

### Where the moat is thin

- **No relationships, no clearance.** The moat is all-technical; if a prime *does*
  commit to the real-time rebuild, the only defence is speed + head start. Move fast;
  the accreditation head start partly offsets.
- **Solo-founder key-person risk.** The earned secret offsets it (the moat *is* the
  founder) but a credible team plan is needed.
- **"Defence company" caps the TAM story.** Chosen deliberately; carry the expansion map
  so investors see the ceiling is high.
- **Canadian-controlled status** is a moat for *Canadian* defence contracts but a
  *constraint* on US capital and ITAR / CGP. Decide early.

### Three next-level moves

1. **Instrument the demo to *generate* the corpus.** Every run on live Victoria AIS
   produces labeled re-ID events — the demo is the flywheel's first turn. Make that
   visible to investors.
2. **Write the "why Palantir won't" memo now.** Spine of the investor narrative *and*
   PRC-2 (novelty / 20 pts). One artifact, two payoffs.
3. **Land a second non-DND design partner** (port authority, Transport Canada, a
   fisheries body). Proves modality-onboarding speed, seeds a second corpus, and blunts
   the single-buyer concern of a defence-only pitch.
