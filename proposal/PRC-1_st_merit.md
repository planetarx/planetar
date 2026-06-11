# PRC-1 — Scientific & Technological Merit

> **Field cap:** 3,000 characters.
> **Score:** 10 pts. (a) Sound S/T evidence + (b) State-of-the-art. Both = 10; one = 5.

---

## Draft (workspace markdown — strip headings before submission)

**(a) Sound S/T evidence.** The approach rests on peer-reviewed deep-learning method, working implementations, and reproducible engineering. *Learned multimodal fusion:* shared-embedding cross-modal representation [I1, I2], attention over concurrent-observation sets [I3], metric-learning re-identification [I4], and joint-embedding self-supervision [I6] are established methods, which the 1a composes for maritime dark-vessel re-ID. *Semi-supervised deep learning on continuous unlabelled streams:* the applicant co-authored ORCA-SLANG (Interspeech 2021) [A2] and Sattar et al. (PacRim 2011, on Ocean Networks Canada data) [A1] — the scarce-label, continuous-stream regime the maritime model works in. *Calibrated uncertainty:* per-modality evidential heads [I5] with conformal prediction [E1, E2] for distribution-free coverage. *Entity resolution with provenance:* the applicant's ~18-month entity-resolution implementation maps cross-modal observation to identity with lineage. *Dark-vessel tractability:* public DoD-component precedent — DIU xView3 [D1]; Park et al., *Science Advances* 2020 [D2] — establishes AI advantage over classical baselines. *Auditory representation learning:* Ness, Walters, Lyon (2012) [A3] (Walters/Lyon at Google Research), relevant to the acoustic encoder. *Real-time / edge engineering:* a provenance-tracked substrate runs the model on commodity hardware (SWaP), measured and reproducible (`docs/benchmark-2026-04-27.md`).

**(b) State-of-the-art.** The model advances current practice on the axes CH13 names. *Learned, adaptive fusion* replaces rule-based aggregation, and adapts at the edge via a runtime-rewritable perception graph [H1]. *Self-supervision from AIS co-occurrence* removes the scarce-label barrier that blocks supervised maritime fusion — to the applicant's knowledge new in this domain. *Uncertainty and explainability are system properties* — propagated evidential uncertainty, conformal coverage, and an attention-grounded causal evidence chain accreditable for operator trust, rather than post-hoc XAI. *Acoustic methodology:* the applicant's archive-scale semi-supervised hydrophone ML [A1, A2] extends from cetacean-call to vessel-signature discrimination, a structurally identical learning problem.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars. Trim Tier-2 evidence bullets if over.

## Cross-references to workspace

- All bracketed citations resolve in `06-REFERENCES.md`.
- Latency claim: `03-ARCHITECTURE.md` Layer 2.
- Identity-resolution scale: `03-ARCHITECTURE.md` Layer 4 + agent-1 audit.
- doibio / entity resolution (patent no longer cited inline in PRC-1): `03-ARCHITECTURE.md` Layer 4 + `04-PORTFOLIO.md` Tier 1B.
