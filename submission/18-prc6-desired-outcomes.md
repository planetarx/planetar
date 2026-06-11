# Field 18 — PRC-6: Alignment of Desired Outcomes

**Form section:** Component 1a → PRC-6
**Cap:** 3000 characters
**LOCAL CHAR COUNT:** 2448 (cap 3000)
**Source:** `proposal/PRC-6_desired_outcomes.md` — RE-CENTERED 2026-05-30 (learned-fusion-model thesis); blank lines stripped per DIP rule.
**Status:** ✅ READY (re-centered)

## Paste-protocol notes

- Blank lines stripped (they count toward the DIP cap). Single newline between paragraphs.
- Markdown emphasis removed; (a)/(b)/(c) and (i)/(ii) labels kept as plain text.
- After paste, DIP's counter should read ~2448 (±2 for newline encoding).

--- PASTE THIS BELOW ---
Planetar covers all five of the Challenge's desired outcomes through one learned cross-modal fusion model and the substrate it runs on:
(1) Spatiotemporal alignment, uncertainty propagation, confidence scoring across modalities. The model encodes each modality and aligns observations in a shared spatiotemporal embedding — alignment is learned, not hand-keyed. Per-modality evidential heads propagate uncertainty through the fusion, and the fused identity score is calibrated by conformal prediction for distribution-free coverage. Deep-learning alignment, propagated uncertainty, and calibrated confidence — exactly as specified. Covered.
(2) Entity resolution + dynamic knowledge graph for persistent cross-domain tracking. Resolved observations populate a dynamic vessel-entity graph that is a substantial development beyond the applicant's prior named-inventor patents (US 10,936,582, US 11,442,952; Salesforce-assigned) and the applicant's ~18-month implementation: canonical Vessel ↔ many identifications (MMSI, IMO, RF / acoustic signature, hull-OCR, report mentions) ↔ provenance records. Covered.
(3) Policy-aware fusion with provenance tracking, full lineage across classification levels. Every observation and every inference carries source and causation_id; outputs are traceable to inputs bit-for-bit, replayable from any historical cursor. Classification-level metadata travels with each item and consumers filter by policy at access time (Protected B at 1a; a multi-level design path). Lineage is structural, not bolted on. Covered.
(4) Scalable real-time pipelines with explainable outputs for operator trust. The model runs on a provenance-tracked substrate on commodity hardware; explainability is native — attention weights over modalities plus click-through from any output back to its raw inputs — and a human analyst is a first-class participant whose adjudication calibrates trust and feeds an active-learning loop. Covered.
(5) SWaP and compute limits incorporated for edge deployment. The substrate is a small-footprint C binary + zero-copy envelope + browser shell, running on laptop-class commodity Linux — no FPGA, no kernel-bypass NIC, no specialised silicon; and the model's perception front-end is a runtime-rewritable MediaPipe graph that adapts networks to power / threat limits — constraint-aware by construction, addressing the Edge Fusion for Tactical Units example. Covered.
All five desired outcomes covered.
--- END PASTE ---
