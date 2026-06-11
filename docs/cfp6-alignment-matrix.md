# CFP6 CH13 — clause-by-clause alignment matrix

Reviewer-facing traceability: every load-bearing phrase in the **verbatim challenge text** mapped to a concrete planetar element and the evidence that backs it (repo path / measurement / citation). This is the source-of-truth for `MC-2_alignment.md` and `PRC-6_desired_outcomes.md`; keep them consistent with this file.

Two threads added 2026-05-30: **CI** = Collaborative Intelligence (human-in-the-loop, CSCW lineage `[G1–G7]`, anchored to the applicant's own peer-reviewed Orchive `[A6a]/[A6b]/[A9]`); **MP** = MediaPipe on-device perception `[H1]`, kept deliberately modest and **proposed** (a few years' experimentation; all planetar usage is 1a R&D). Both threads' verify items are resolved (see end).

---

## 1. Challenge statement clauses

| CFP clause (verbatim) | planetar element | Evidence |
|---|---|---|
| "fuse heterogeneous multi-domain data streams" | A **learned cross-modal fusion model** encodes six typed modalities (AIS, SAR, EO, acoustic, RF, text) into a shared embedding | the model (1a R&D) on a built substrate: `planetar-broker`, `zmesg`, `planetar-{ais,sat,eo,acoustic}` + new RF/text encoders |
| "real-time" | ns SHM bus p50 80–140 ns; deployment-realistic TCP path p50 34 µs | `docs/benchmark-2026-04-27.md` |
| "explainable" | causation-id chain on every output envelope; one-click traversal to raw inputs | `03-ARCHITECTURE.md` L1, L5 |
| "policy-aware" | per-message `source` + extensible flags; **policy filter applied at subscription time**; classification-level metadata travels in the envelope | `03-ARCHITECTURE.md` L1–L2 |
| "situational awareness" | 5-viewer analyst shell (map / timeline / entity-card / waveform / channel) | `planetar-ui` (predecessor `sales4`) |
| "operational decision-making" | **CI** — operator adjudication is itself a bus message; the human+AI decision is shared, not delegated | `03-ARCHITECTURE.md` L5; `[G1–G7]` |
| "reduce system vulnerabilities" | single auditable spine replaces brittle siloed point-to-point glue; tamper-evident WAL lineage; WAL-replay degraded-mode resilience; the dark-vessel demo closes an evasion/surveillance gap | `03-ARCHITECTURE.md` L2 (WAL); flagship demo |
| "increase speed of decision making" | ns cross-modal correlation + a tightened human-in-the-loop adjudication loop (**CI**) | `03-ARCHITECTURE.md` L5 |

---

## 2. Essential Outcome (mandatory, pass/fail)

| CFP requirement | planetar element | Evidence |
|---|---|---|
| "aggregate, ingest, fuse, and generate outputs from at least two (2) heterogeneous data types … to produce … classifications, detection, correlations" | The learned fusion model fuses **six** types spanning *sensor / text / RF* (AIS, SAR, EO, acoustic, RF, text) → `vessel.v1.ReIDCandidate` = detection + correlation + classification with calibrated confidence | Four detectors shipped + broker-integrated; the learned fusion model + RF/text encoders are the M2–M4 R&D. **2-type bar cleared with margin, against the challenge's own example.** |

---

## 3. Desired Outcomes (scored — PRC-6, 15 pts, target 100%)

| # | Desired outcome | planetar element | Evidence |
|---|---|---|---|
| 1 | Spatiotemporal alignment, uncertainty propagation, confidence scoring | **Learned alignment** in the fusion model's shared embedding; per-modality evidential uncertainty propagated + conformal calibration | `[I1, I3, I5]`, `[E1, E2]` |
| 2 | Entity resolution + dynamic knowledge graph | patent-backed Party-model graph retyped for vessels; `planetar-ontology` P1–P5, 30 tests | `[A8a]`, `doibio` |
| 3 | Policy-aware fusion + provenance + lineage across classification levels | `correlation_id`/`causation_id`/`source`; CRC32 append-only WAL; subscription-time policy filter | `03-ARCHITECTURE.md` L1–L2 |
| 4 | Scalable real-time pipelines + explainable outputs for operator trust | ns bus + click-through explainability **+ CI layer: operator adjudication as a bus message, conformal-calibrated trust, active-learning loop** | `03-ARCHITECTURE.md` L5; `[G1–G7]` |
| 5 | SWaP + compute limits for edge deployment | ~1.2k-LOC C bus + zero-copy envelope + browser shell **+ MediaPipe on-device perception + agentic graph adaptation** | `[H1]`; `03-ARCHITECTURE.md` L3 |

---

## 4. Application examples (CFP "examples … but not limited to")

| Example | Alignment | planetar element |
|---|---|---|
| **Maritime Task Group Operations** — sonar + RF + visual, anomaly detection, uncertainty + explainable | **PRIMARY** | dark-vessel re-ID flagship: acoustic + EO + SAR + AIS-gap fusion, conformal confidence, causation-chain explainability |
| Joint ISR Fusion for Arctic Operations — sat + RF + telemetry | secondary | same bus + entity model, different deployment geography |
| **Edge Fusion for Tactical Units** — audio + video + sensor on wearables, degraded connectivity | **MP fit** | **MediaPipe on-device perception** + agentic graph adaptation + edge SWaP profile + offline WAL operation |
| Real-Time Threat Assessment (multi-domain) — EO + SIGINT + text | composability claim | add one ingress adapter + one schema + one viewer; no monolith change |
| Airborne Multi-Sensor Platforms — radar + EO/IR + telemetry | future potential (PRC-2c) | same composition pattern, new modalities |

---

## 5. Under-weighted threads to re-surface (CFP emphasis vs. current draft)

The verbatim challenge text foregrounds four ideas the current narrative under-weights relative to dark-vessel detection. Re-surface explicitly in MC-2 / PRC-3:

| CFP thread | planetar answer |
|---|---|
| "reduce system vulnerabilities" | single auditable bus vs. siloed integrations; tamper-evident lineage; degraded-mode WAL replay; dark-vessel detection closes an adversary-evasion gap |
| "increase speed of decision making" | ns correlation + **CI** shared human+AI decision (AI correlates at volume; human adjudicates the high-stakes call; shell collapses the latency between) |
| "secure integration across classification levels … contested/degraded environments" | policy filter at subscription; 1a is Protected B with a multi-level design path; offline edge operation on MediaPipe |
| "learned, **adaptive** fusion across modalities rather than **static aggregation**" (CFP's own words) | agentic **MediaPipe** graph rewriting (constraint-aware perception) + calibrated cross-modal fusion — adaptive by construction |

---

## `[VERIFY]` items — resolved 2026-05-30

1. **CI `[A1]` — RESOLVED.** Dedicated annotation-tooling paper is **[A9]** (*Chants and orcas*, ACM MM-Semantics 2008); CI is a named contribution. Orchive facts verified from `ness2013thesis_v7.pdf` + `[A6b]`: 20,000-hr/30-yr archive, 18,000+ annotations, expert + citizen-science casual-game interfaces, *Intelligence Augmentation* (§2.3) + *Citizen Science* (§2.4) thesis chapters.
2. **MP `[B]` — RESOLVED.** No authored MediaPipe product under `~/github/sness23/` (`funchromium` = Chromium checkout; `sness23/mediapipe` = upstream fork, Copybara-only commits). Per applicant: keep MediaPipe **proposed and modest** — a few years of hands-on experimentation, practitioner familiarity only; no product/LOC/channel claim.
3. **MP agentic `[B]` — RESOLVED.** Proposed, not prototyped — a clean TRL-1–2 novelty deliverable for the 1a.

No external inputs remain outstanding for these two threads — the budgeted-narrative pass (PRC-2/4/5/6, MC-2) can proceed.
