# 01 — The Challenge: CH13 & Scoring Rubric

Ported from v1. CH13 is unchanged between submissions; the rubric is the same regardless of which architecture wins it.

## Challenge statement (verbatim)

> "The Department of National Defence and Canadian Armed Forces (DND/CAF) are seeking innovative AI (Artificial Intelligence)-driven solutions that fuse heterogeneous multi-domain data streams to provide real-time, explainable, and policy-aware situational awareness for operational decision-making."

## Essential outcome (mandatory, pass/fail)

> "Deliver an AI model that can aggregate, ingest, fuse, and generate outputs from at least two (2) heterogeneous data types (e.g. sensor, text, RF) to produce output metrics and measures (e.g. classifications, detection, correlations)."

planetar fuses **four production modalities** at the bus level (AIS, SAR, EO, hydrophone) plus a fifth stub topic (non-AIS RF). The two-modality bar is cleared with margin.

## Desired outcomes (PRC-6 driver, scored 15 / 100)

| # | Outcome | planetar mapping |
|---|---|---|
| 1 | Spatiotemporal alignment, uncertainty propagation, confidence scoring across modalities | ns-stamped envelopes; conformal prediction on fused scores; log-score decomposition (`crank3` pattern) |
| 2 | Entity resolution + dynamic knowledge graph for persistent cross-domain object tracking | Patent-backed Party-model graph (Layer 4); doibio's identity-resolution engine retyped for vessels |
| 3 | Policy-aware fusion with provenance tracking and full lineage | Per-message `correlation_id` / `causation_id` / `source`; CRC32 WAL; every output traceable to inputs |
| 4 | Scalable real-time fusion pipelines + explainable outputs for operator trust | Measured 80–140 ns p50 SHM bus; viewer click-through from output to causal envelope |
| 5 | SWaP and compute limits incorporated for edge deployment | ~1.7k-line C broker + zero-copy envelope + browser shell — runs on a laptop today |

Coverage target: 100% (15 pts).

## Application examples (verbatim from CH13)

- Joint ISR Fusion for **Arctic Operations** — sat imagery + RF + telemetry.
- Real-Time Threat Assessment in Multi-Domain Battlespace — EO + SIGINT + text intel.
- Edge Fusion for Tactical Units — audio + video + sensor on wearables under degraded comms.
- **Maritime Task Group Operations** — sonar + RF + visual feeds, anomaly detection with uncertainty + explainable outputs. ← **target**.
- Airborne Multi-Sensor Platforms — radar + EO/IR + telemetry for stealth / spoofed asset tracking.

planetar is positioned against **Maritime Task Group Operations** primarily, with **Arctic Operations** as a secondary alignment claim (same architecture, different deployment geography).

## Component 1a parameters

| Parameter | Value |
|---|---|
| TRL range | 1–3 |
| Max funding | $250,000 CAD |
| Max duration | 6 months |
| Phase intent | "Conceive" — prove concept is possible |
| Government Furnished Property | **NOT available at 1a** |
| Classified proposals | **NOT accepted** at 1a (Protected B or lower) |
| Security clearance | Not required |

## Scoring rubric — 100 pts total, ≥70 required to be responsive

### Screening (pass/fail)

**SC-1 Financial.** ≤$250K AND first-half milestones ≤70% of total. Budget cannot be front-loaded.

### Mandatory criteria (pass/fail)

**MC-1 Current TRL + R&D done.** (a) Accurately identify TRL (1, 2, or 3). (b) Describe R&D activities already done to reach that TRL. *Locked phrasing in 08-OPEN-QUESTIONS Q1.*

**MC-2 Alignment with S&T Challenge.** Describe the solution + its scientific/technological basis + justify how it meets each Essential Outcome.

### Point-rated criteria (100 pts, ≥70 required)

| # | Criterion | Pts | Sub-criteria |
|---|---|---|---|
| PRC-1 | S/T Merit | 10 | (a) Sound S/T evidence · (b) State-of-the-art. Both = 10, one = 5. |
| PRC-2 | Novel & Innovative | 20 | (a) New knowledge/tech · (b) Enhanced vs SOTA · (c) Future potential. All = 20, two = 15, one = 5. |
| PRC-3 | Impact | 20 | (a) Solves a gap · (b) Enhances S/T capability · (c) Matures the field. All = 20, two = 15, one = 5. |
| PRC-4 | Feasibility | 20 | (a) Achievable · (b) Well-reasoned · (c) Risks + mitigations. All = 20, two = 15, one = 5. |
| PRC-5 | GBA Plus | 5 | Applied to the technical solution. Done = 5, planned = 2, none = 0. |
| PRC-6 | Desired outcomes | 15 | 100% = 15, 50–99% = 10, <50% = 5, none = 0. |
| PRC-7 | Cost alignment | 10 | Realistic, milestone-aligned, proportional. Full = 10, gaps = 5, misaligned = 0. |
| | **Total** | **100** | **≥70 required** |

### Character-count caps (per narrative)

| Field | Cap |
|---|---|
| MC-1, MC-2 | 3,000 chars each |
| PRC-1 … PRC-6 | 3,000 chars each |
| PRC-7 | Tables only |

Total narrative ≈ 24,000 chars ≈ 4,000 words. **A short, surgical bid, not a 30-page document.** Every claim has to earn its line.

## Submission mechanics

1. **Defence Innovation Portal (DIP)** is the only accepted channel.
2. **Registration:** ≥48h before close. DIP submission is in for v1; carries forward.
3. **Electronic Proposal Form** — character-capped fields + financial tables.
4. Submissions are final once submitted. "Replacement submission" path requires the prior reference number.
5. **Alternative bids allowed** if substantively different. *We are not submitting both v1 and planetar; v1 is reference only.*
6. **No attachments** unless requested. Anything cited externally must be open-source-accessible (USPTO record for the patent counts; an internal PDF does not).
7. **No classified content.** Protected B or lower.

## After submission

- Evaluation: typically 3–6 months post-close.
- Pre-qualified proposals enter a 180-day pool.
- Contracts negotiated against an SOW derived from the winning proposal.
- Canada may negotiate milestones / budget / deliverables pre-award.
- **Audit right: 6 years post-contract.** Every claim in the bid must survive that window.

## Implications for planetar specifically

- The 24k-char narrative budget is tight. Architecture novelty + measured numbers + patent + ONC history each need to land in ~300–500 chars.
- The "audit right: 6 years" is the reason every claim in `02-STRATEGY.md` and `03-ARCHITECTURE.md` is grounded in code or measurement, not aspiration.
- 1a is concept-prove; the 110 ns measurement is allowed to be on a developer machine; production hardening is a 1b deliverable, not a 1a one.
- "Maritime Task Group Operations" is the example most directly served. The proposal will reference this example by name in MC-2.
