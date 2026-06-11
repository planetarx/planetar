# Field 07 — Project Overview

**Form section:** Component 1a → 2 - Project Description → C
**Field label:** `*Provide a project overview, which is a synopsis of project's S/T Merit, Novelty and Innovation, Impact, Feasibility and Approach.`
**Cap:** **3,000 characters**
**Type:** Text area
**Status:** ✅ RE-CENTERED 2026-05-30 (learned-fusion-model thesis)

## Audience

Reviewer's first detailed read before the PRC fields. Citations bare (the PRCs carry their own evidence). Each paragraph maps to a PRC.

--- PASTE THIS BELOW ---
Planetar is a learned cross-modal AI model for maritime situational awareness. It encodes heterogeneous sensor and text streams into a shared representation where one vessel's observations resolve to a single, calibrated, explainable identity — even when the vessel disables its Automatic Identification System (AIS) to go dark. The Component 1a flagship is dark-vessel detection; the supporting real-time, provenance-tracked data platform is already built and open-source.
Scientific and Technical Merit (PRC-1): The core method is a self-supervised cross-modal fusion model. While a vessel broadcasts AIS, its identity labels the concurrent synthetic-aperture radar (SAR), electro-optical (EO), acoustic, radio-frequency, and text observations; the model learns that association and applies it to re-identify vessels once AIS is dark — dissolving the scarce-label barrier that blocks supervised maritime fusion. Per-modality uncertainty is propagated and calibrated with conformal prediction for distribution-free confidence bounds. The applicant's peer-reviewed semi-supervised deep learning on unlabelled acoustic archives is direct evidence the approach is achievable.
Novelty and Innovation (PRC-2): Self-supervised, learned cross-modal dark-vessel re-identification across this modality set is, to the applicant's knowledge, absent from the literature. Fusion is learned and adaptive — not rule-based aggregation — with attention-grounded explanations and a human analyst in the loop.
Impact (PRC-3): Closes the operationally costly AIS-off capability gap (illegal fishing, sanctions evasion, Arctic sovereignty incursions). Restoring track continuity on vessels that evade monitoring reduces a surveillance vulnerability and speeds decision-making. Open-source on commodity hardware it is addressable for Canadian Armed Forces (CAF) interoperability with allies.
Feasibility and Approach (PRC-4): The work runs over two milestone stages on public data. Milestone 1 hardens the data substrate and the six-modality ingest (adding text and RF), builds the per-modality encoders and the AIS co-occurrence training set, and trains the self-supervised cross-modal fusion model. Milestone 2 calibrates and evaluates uncertainty on AIS-off re-identification, builds the explainable human-in-the-loop analyst surface, and delivers a replayable Salish Sea demonstration plus a live system at planetar.ca the evaluator can operate, with public-benchmark evaluation. Solo Zax Analytics execution. No government-furnished property, no field deployments, no classified content.
--- END PASTE ---

## Notes

- A synopsis — claims assertively here; the PRC fields carry citations.
- If DIP rejects as over 3,000: trim the trailing deliverable list in the Feasibility paragraph first.
