# PRC-4 — Feasibility & Approach

> **Field cap:** 3,000 characters.
> **Score:** 20 pts. (a) Achievable + (b) Well-reasoned + (c) Risks + mitigations. All three = 20.

---

## Draft (workspace markdown — strip headings before submission)

**(a) Achievable in practice.** Working foundations: bus (`planetar-broker`; predecessor `zbroker0` measured p50 = 80–140 ns / p99 = 400–900 ns SHM, 2026-04-27, `docs/benchmark-2026-04-27.md`); envelope (`zmesg` — UUIDv7, ns timestamps, full provenance); entity graph (`doibio` — ~20 k LOC, 35 schemas, 630-line identity-resolution engine, 18 mo iteration); analyst shell (`planetar-ui`; predecessor `sales4`, React 19 + WebSocket + µs instrumentation). The 1a integrates these and adds cross-modal vessel-ID matchers — the new R&D.

All five modalities have public coverage (per `05-DATASETS.md`): AIS (MarineCadastre / GFW); SAR (Sentinel-1 + xView3 [D1]); EO (Singapore Maritime / MODS); hydrophone (ONC, ShipsEar [D4], DeepShip [D5]); RF (stub). No GFP, no classified data, no field deployments. Compute fits ~$18 K cloud over 6 months (`docs/compute-estimate.md`).

Solo execution is realistic: applicant has shipped comparable systems (CRANK [A7]; Salesforce data-integration patent [A8a]; `doibio` 18-month build) and authored the spine.

**(b) Well-reasoned approach.** Six monthly milestones (per `07-TIMELINE.md`):

- *M1 — Bus hardening:* harden `planetar-broker` v0.1.0 (supersedes predecessor `zbroker0`); reproducible benchmark; CI.
- *M2 — Ingress:* four ingresses on public data; WAL cold storage; replay-from-cursor verified.
- *M3 — Detectors:* five typed-bus detectors with causation lineage — `ais.gap` heuristic, `sar.chip` (xView3-derived CFAR + CNN), `eo.chip` (fine-tuned public detector), `acoustic.event` (ORCA-SLANG semi-supervised), `vessel.ReIDCandidate` (cross-modal fusion).
- *M4 — Entity graph + research:* Party-model retyped for vessels; cross-modal re-ID with conformal-calibrated confidence and full causation lineage. **Primary research deliverable.**
- *M5 — Shell viewers:* map, timeline, entity-card, waveform, channel — each a bus consumer.
- *M6 — Demo + report:* end-to-end Salish-Sea synthetic dark-event scenario replayable from WAL; public-benchmark evaluation; 1b proposal package.

Each milestone has measurable exit criteria; no frontier-scale model training; all detectors are fine-tunes of public baselines or extensions of applicant's published methodology.

**(c) Risks + mitigations.**

- *SAR baseline weak on Arctic imagery (medium):* demo is Salish Sea; Arctic claim is same-architecture-different-deployment.
- *Hydrophone vessel-transit ground truth sparse (medium):* fall back to general acoustic event detection per Sattar/ORCA-SLANG; vessel discrimination via ShipsEar/DeepShip retains modality without overclaim.
- *Re-ID calibration underperforms (low):* conformal prediction provides distribution-free coverage as a back-stop — guaranteed even if the score is weak.
- *Shell exceeds M5 budget (medium):* map + timeline + entity-card prioritised; waveform ships as v0.1 raw renderer.
- *Solo-founder sickness / burnout (medium):* M6 buffer is scope-cut insurance; if schedule slips, scope is cut (e.g., waveform viewer to v0.1) before timeline is extended.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars.

## Cross-references

- Detailed M1–M6: `07-TIMELINE.md` execution calendar.
- Risk register: `07-TIMELINE.md` end of file.
- Code already-existing: `04-PORTFOLIO.md` "Working code" + `02-STRATEGY.md` "What's actually built today."
- Datasets: `05-DATASETS.md`.
