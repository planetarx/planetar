# MC-2 — Alignment with the S&T Challenge

> **Field cap:** 3,000 characters.
> **Pass/fail.** Must describe (i) the solution, (ii) its scientific/technological basis, (iii) how it meets each Essential Outcome.

---

## Draft (workspace markdown — strip headings before submission)

**Solution.** *Planetar* is a **learned cross-modal fusion model** for maritime domain awareness. Six heterogeneous streams — AIS (kinematic + text fields), Sentinel-1 SAR, electro-optical, passive acoustic, RF emissions, and textual maritime reports (notices-to-mariners / port-state / OSINT) — are encoded into a shared spatiotemporal embedding in which observations of the same vessel resolve to one calibrated, uncertainty-scored identity, even after its AIS beacon goes dark. The model is trained **self-supervised**: during AIS-on periods, a broadcasting vessel's identity labels the concurrent SAR/EO/acoustic/RF/text observations in its space-time neighborhood; the model learns that cross-modal association and applies it to re-identify AIS-off ("dark") vessels — CH13's *Maritime Task Group Operations* example. Every output carries calibrated confidence and an attention-weighted evidence chain, with a human analyst in the loop. This serves the CAF Digital Campaign Plan and DND/CAF AI Strategy goals of learned, explainable multi-domain fusion for ISR and C2.

**Scientific and technological basis.** (i) Deep multimodal representation learning and learned association — per-modality encoders projected to a shared metric space, a transformer fusion head over concurrent observations, trained self-/semi-supervised on AIS co-occurrence [I1, I2, I3, I6]. The applicant's peer-reviewed semi-supervised deep learning on unlabeled acoustic archives (ORCA-SLANG, Interspeech 2021 [A2]; Sattar et al. PacRim 2011, 95 % on Ocean Networks Canada data [A1]) is direct evidence the approach is achievable. (ii) Uncertainty propagation — per-modality evidential heads [I5] carried through fusion, then conformal calibration [E1, E2] for distribution-free coverage. (iii) Entity resolution + dynamic knowledge graph for persistent cross-domain tracking. (iv) Native explainability and provenance — each inference renders its causal evidence for operator trust and accreditation. A working real-time, provenance-tracked substrate the applicant has already built makes the model deployable at the edge (SWaP) and bit-exact replayable.

**Essential Outcome compliance.** The Essential Outcome requires an AI model fusing ≥ 2 heterogeneous types (e.g. *sensor, text, RF*) into classifications / detections / correlations. Planetar's model fuses **six** types spanning exactly sensor, text, and RF, producing: detection (vessel present), classification (vessel class), and correlation (cross-modal identity / dark-vessel re-ID) with calibrated confidence and per-modality evidence. Fusion is **learned and adaptive** — a trained cross-modal association model, not rule-based aggregation — and adapts its perception front-end to SWaP and threat conditions at the edge. The two-type bar is cleared with margin, against the challenge's own *sensor / text / RF* example.

---

## Char-count budget

Target: ≤ 2,950 chars. Trim modality bullet list to compact prose if over.

## Cross-references to workspace

- Architecture lineages: `02-STRATEGY.md` + `03-ARCHITECTURE.md` Layers 1–5.
- Bus measurement: `03-ARCHITECTURE.md` Layer 2.
- Entity graph (patent no longer cited inline in MC-2): `03-ARCHITECTURE.md` Layer 4 + `04-PORTFOLIO.md` Tier 1B.
- Datasets: `05-DATASETS.md`.
- References: `06-REFERENCES.md` [A1, A2, A8a, B1a, D1, D4, D5].
