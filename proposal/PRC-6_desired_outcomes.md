# PRC-6 — Desired Outcomes coverage

> **Field cap:** 3,000 characters.
> **Score:** 15 pts. 100 % = 15, 50–99 % = 10, < 50 % = 5, none = 0. Target: **all five**.

---

## Draft (workspace markdown — strip headings before submission)

Planetar covers all five CH13 desired outcomes through one learned cross-modal fusion model and the substrate it runs on:

**(1) Spatiotemporal alignment, uncertainty propagation, confidence scoring across modalities.** The model encodes each modality and aligns observations in a shared spatiotemporal embedding — alignment is *learned*, not hand-keyed. Per-modality evidential heads [I5] propagate uncertainty through the fusion, and the fused identity score is calibrated by **conformal prediction** [E1, E2] for distribution-free coverage. Deep-learning alignment, propagated uncertainty, and calibrated confidence — exactly as specified. **Covered.**

**(2) Entity resolution + dynamic knowledge graph for persistent cross-domain tracking.** Resolved observations populate a dynamic vessel-entity graph — a substantial development beyond the integrated-entity-view architecture of the applicant's prior named-inventor patents (**US 10,936,582**, **US 11,442,952** [A8a, A8b]; Salesforce-assigned) and the applicant's ~18-month implementation: canonical Vessel ↔ many identifications (MMSI, IMO, RF / acoustic signature, hull-OCR, report mentions) ↔ provenance records. **Covered.**

**(3) Policy-aware fusion with provenance tracking, full lineage across classification levels.** Every observation and every inference carries `source` and `causation_id`; outputs are traceable to inputs bit-for-bit, replayable from any historical cursor. Classification-level metadata travels with each item and consumers filter by policy at access time (Protected B at 1a; a multi-level design path). Lineage is structural, not bolted on. **Covered.**

**(4) Scalable real-time pipelines with explainable outputs for operator trust.** The model runs on a provenance-tracked substrate on commodity hardware; explainability is native — attention weights over modalities plus click-through from any output back to its raw inputs — and a human analyst is a first-class participant whose adjudication calibrates trust and feeds an active-learning loop [A6a][A9][G4]. **Covered.**

**(5) SWaP and compute limits incorporated for edge deployment.** The substrate is a small-footprint C binary + zero-copy envelope + browser shell, running on laptop-class commodity Linux — no FPGA, no kernel-bypass NIC, no specialised silicon; and the model's perception front-end is a runtime-rewritable MediaPipe graph [H1] that adapts networks to power / threat limits — constraint-aware by construction, addressing the *Edge Fusion for Tactical Units* example. **Covered.**

All five desired outcomes covered.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars.

## Cross-references

- Outcome → implementation map: `01-CHALLENGE.md` table.
- Bus measurement: `03-ARCHITECTURE.md` Layer 2.
- Patent + doibio: `04-PORTFOLIO.md` Tier 1B + `03-ARCHITECTURE.md` Layer 4.
- Lineage: `03-ARCHITECTURE.md` Layer 1 + Layer 2 (WAL).
