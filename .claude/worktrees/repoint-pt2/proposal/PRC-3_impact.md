# PRC-3 — Impact

> **Field cap:** 3,000 characters.
> **Score:** 20 pts. (a) Solves a gap + (b) Enhances S/T capability + (c) Matures the field. All three = 20.

---

## Draft (workspace markdown — strip headings before submission)

**(a) Addresses a stated capability gap.** CH13 names the gap: fragmented multi-domain streams, siloed modalities, rule-based aggregation, opaque outputs. Within that, the AIS-off ("dark vessel") problem in CH13's *Maritime Task Group Operations* example is operationally costly today — illegal fishing, sanctions evasion, ship-to-ship transfers, and Arctic sovereignty incursions all depend on AIS being disabled. Current ISR handles this with stovepiped per-modality pipelines, ms-class integration, and post-hoc explainability bolted on. planetar collapses these into one nanosecond bus, one provenance-tracked entity graph, and one composable viewer shell — a structural change to the gap. CH13 also identifies a sovereign-capability gap; planetar's stack is open-source-replicable on commodity Linux and grounded in applicant-named US Patent 10,936,582 [A8a] — addressable as a Canadian-IP-grounded component for CAF interop with allies.

**(b) Enhances S&T capability.**

- *Nanosecond-class messaging applied to defence ISR.* LMAX-Disruptor-lineage architectures [B1a] are common in finance but rare in defence ISR; the p50 = 80–140 ns measurement on the predecessor `zbroker0` (reproduced 2026-04-27, `docs/benchmark-2026-04-27.md`; now succeeded by `planetar-broker`) enables concurrent real-time cross-modal hypothesis adjudication — a regime not reachable with conventional ms-class buses.
- *Productionized patent-backed entity resolution in the maritime domain.* US 10,936,582 + 18-month `doibio` reference implementation [`identity-resolution.ts`, 630 lines] retypes from research-entity to vessel-entity context while preserving the patented mechanism.
- *Methodological transfer.* Semi-supervised deep learning at archive scale on continuous unlabeled hydrophone streams (Sattar et al. 2011 [A1]; ORCA-SLANG 2021 [A2]) extends from cetacean-call detection to vessel acoustic signatures — structurally identical learning problem.
- *Native lineage as an architectural property.* Per-output `causation_id` chains make every detection traceable to its raw inputs in one application — a measurable advance beyond post-hoc XAI.

**(c) Matures the field.**

- *Reusable platform pattern.* The bus + envelope + entity-graph + viewer composition is one architectural pattern that serves CH13's other application examples (Arctic ISR, Airborne Multi-Sensor, Edge Tactical) without redesign; applicant intends to publish this pattern as a generalizable contribution to multi-domain CAF ISR architecture.
- *Lower barrier to defence-grade explainable AI.* Calibrated outputs (conformal prediction [E1]) plus structural lineage address a known barrier to AI accreditation in operational use — outputs are accreditable as a system property, not an afterthought.
- *Durable IP value.* Applicant-controlled code (`doibio`, `planetar-broker`, `zmesg`, `planetar-ui`) and applicant-named patent [A8a] can be published as papers and reference implementations; the 1a output is durable beyond the contract window.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars.

## Cross-references

- Latency: `03-ARCHITECTURE.md` Layer 2.
- Patent + doibio: `04-PORTFOLIO.md` Tier 1B + `03-ARCHITECTURE.md` Layer 4.
- Maturation pattern: `02-STRATEGY.md` "Pivot from v1" table.
