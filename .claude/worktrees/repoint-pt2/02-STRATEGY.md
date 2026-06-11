# 02 — Technical Strategy

## The pitch in one sentence

**planetar** is a **real-time multi-modal situational-awareness platform** — a Slack-style viewer shell for analysts, a Palantir-style provenance-tracked entity graph for re-identification, and an LMAX-Disruptor-inspired **nanosecond message bus** that makes both possible on a single host at edge scale — with the 1a flagship application being **AIS-off dark-vessel detection**.

The bus is the thesis. Everything else — the entity graph, the viewers, the detectors — is a consumer on the bus. That single architectural choice is what lets SAR, AIS, passive acoustic, EO, and non-AIS RF be fused in real time with explainable lineage end-to-end.

---

## Why this is a CH13 answer, not a generic platform pitch

CH13 asks for:

1. **Fusion across at least two heterogeneous data types** (essential outcome). planetar ships with four connected at the bus level: AIS (text/kinematic), SAR (imagery), hydrophone (audio), EO (imagery). Dark-vessel detection uses all of them.
2. **Spatiotemporal alignment.** The envelope (zmesg) carries nanosecond `created_at_ns` at ingress; the bus preserves ordering; viewers correlate on `(topic, t, entity_id)`.
3. **Entity resolution + dynamic knowledge graph.** Already covered by US 10,936,582 (2021); applicant-named inventor. The graph is a bus consumer with write-back.
4. **Policy-aware provenance with full lineage.** Every envelope carries `correlation_id` / `causation_id` / `source`. Combined with the WAL, every output is traceable to its inputs.
5. **Real-time AI-powered fusion.** The bus sustains p50 = 80–140 ns, p99 = 400–900 ns over 1M-message benchmarks (`docs/benchmark-2026-04-27.md`) — that is not "real-time" by convention, that is *faster than most CPUs can context-switch*. Whatever fusion model we bolt on is bus-bound at zero meaningful overhead.
6. **Explainable outputs for operator trust.** The viewer shell lets the analyst click from a detection back through its causation chain to raw inputs — not as post-hoc explanation, but as the native structure of the system.
7. **SWaP / edge.** The bus is a few-kloc C binary; the envelope is zero-allocation zero-copy; the shell is a web app. The whole thing runs on a laptop today.

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

3. **Re-identify.** The entity-graph consumer fuses gap events with concurrent SAR/EO/acoustic detections in the same spatial neighborhood and publishes `vessel.v1.ReIDCandidate{entity_id, evidence: [msg_id, ...], score}` — each candidate is a graph node backed by the specific input envelopes that produced it.

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
| Lock-free, zero-syscall, zero-copy on the hot path | zbroker0/broker-unified.c — SHM via memfd + SCM_RIGHTS, CAS reserves, eventfd only on wake |
| Durable, recoverable, cross-transport fan-out | WAL with CRC32 + segment rotation; TCP/UDP/SHM all reach all consumers |
| Nanosecond-precision ingress timestamps on every envelope | zmesg.h — `created_at_ns`, `stored_at_ns`, `published_at_ns` as uint64 ns |
| Working Slack-style multi-client shell with µs-precision latency instrumentation | sales4 comms-app + server (React 19 / TypeScript / WebSocket), already microsecond-logged |
| Granted patent covering integrated entity view across distributed systems | US 10,936,582 (2021) |
| Working POC implementation of the patent | doibio: 20k+ LOC, 35 entity schemas, 630-line identity-resolution engine, event-sourced backend, ~18 mo iteration |
| Audit-defensible ONC institutional bond on hydrophone ML | Sattar, Driessen, Tzanetakis, Ness, Page (IEEE PacRim 2011), 95% accuracy on NEPTUNE Canada/ONC hydrophone data, explicit acknowledgement of NEPTUNE/CANARIE support |

All the above exist as code, IP, or peer-reviewed publication **before** the 1a begins. The 1a research question is not "can this be built" — it is "can the cross-modal dark-vessel detection head be wired onto this spine and produce calibrated, explainable outputs on public data in 6 months." That's a well-scoped TRL 2 → 3 advance.

---

## What's actually built today (the doibio POC)

The patent claim is materially stronger because the architecture it covers is already implemented. `~/data/dev/doibio` is an event-sourced filesystem-first entity store the applicant has run for ~18 months. Not a sketch — production-grade for the applicant's own use.

What doibio has shipped:

- **Salesforce-inspired Party model.** Canonical Party (`pty_…`) ↔ many PartyIdentifications (`pid_…`) ↔ Individual / Organization details, with PartySource records tracking origin + import method + quality score. 35 JSON schemas in `vault/_schemas/`.
- **Identity-resolution engine.** `src/lib/identity-resolution.ts`, 630 lines, production-grade. Levenshtein-based fuzzy matching + exact-ID matching; confidence scoring (1.00 exact ID / 0.90 strong identifier / 0.70 fuzzy name / 0.50 institutional); thresholds (merge ≥0.95, link ≥0.80, review ≥0.70, new <0.70). The thresholds are tuned — applicant has run them on real data.
- **Event-sourced backend.** Append-only event log (`vault/_logs/*.md`) is source of truth; SQLite + markdown projections; replay and time-travel queries are first-class. The pattern matches Kafka log compaction; doibio's `docs/RESEARCH-linkedin-kafka-architecture.md` shows the design choice was deliberate, not accidental.
- **Slack-channel UI** (`comms-app/`) wired to entity store via REST + WebSocket, channels per topic, real-time updates via WS broadcast.
- **Schema-first validation** with AJV + git hooks (Node + Python). 35 schemas under version control.
- **Explicit Palantir-Ontology and LinkedIn-Kafka research docs** in `docs/RESEARCH-*.md`. Architecture choices were made with eyes open.

What doibio's POC scaffolding gets retired in planetar:
- 2-second polling for real-time updates → bus subscription (this is exactly what zbroker0 fixes).
- Hardcoded Cohere AI integration → pluggable model layer.
- Stubbed merge command → atomic merge as a typed bus event.
- Single SQLite `channels.db` → channels as bus topics.

**The implication for the proposal narrative.** PRC-2 (novelty) does not say "we have an idea." It says: *"The applicant is named inventor on US Patent 10,936,582 covering the integrated-entity-view architecture. That architecture is implemented in the applicant's working POC (doibio: ~20 k LOC, 35 schemas, working identity-resolution engine, ~18 months of iteration). planetar productionizes the implementation, retypes the entity model for vessels, and binds it to the measured 80–140 ns nanosecond bus."* Three steps, each independently verifiable. This is the strongest patent-grounded novelty claim available within an honest framing.

---

## Honesty principles (inherited from v1)

1. **Third-party code is framed as power-user work, not authorship.** Aeron, Protenix, chai-lab, boltz, chemeleon, ChimeraX are other people's code. planetar's bus, envelope, and shell are the applicant's.
2. **No "we implemented JEPA" claims.** JEPA-style fusion is a candidate for the detection head — if we ship it, we ship it as an adapted public implementation with proper attribution. Feasibility is not claimed on internals we haven't built.
3. **TRL honesty.** Bus + envelope + shell + patent are TRL 3–4 today. Cross-modal dark-vessel fusion is TRL 2 today. The 1a advances the fusion component to TRL 3 on the existing spine. Framing in MC-1 matches this precisely.
4. **No marketing adjectives** in the narrative. "Nanosecond" is a measurement, not a slogan. "Palantir-like" and "Slack-style" are used to anchor reviewer intuition — the proposal will cite them once and move on to specific architectural claims.
5. **All novelty claims must survive a post-award audit.** See 04-PORTFOLIO (TODO) for the specific citable anchors.

---

## Pivot from v1, explicitly

| Dimension | v1 (MAIA-MD) | v2 (planetar) |
|---|---|---|
| Core thesis | A single JEPA fusion model | A nanosecond bus with fusion as a consumer |
| TRL 3 evidence | Adaptation from adjacent published work | A reproduced, measured broker + working shell + patent |
| Novelty anchor | Architectural coupling of JEPA + provenance graph | The bus latency itself + the platform composition |
| Demo scope | Algorithmic output report | End-to-end running system the reviewer can open in a browser |
| "Explainability" | Post-hoc, through provenance tags | Native — viewer clicks from output to causal inputs |
| Fit to solo 1a | Risky (JEPA-scale training) | Safe (wiring known components) |
| Differentiation | Yet-another-fusion-architecture | "They built Palantir on a Disruptor. In C. Solo." |

The v2 story is **more defensible under audit, more demonstrable in 6 months, and more distinctive in the pool of CH13 bidders.**
