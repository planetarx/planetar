# PRC-2 — Novel & Innovative

> **Field cap:** 3,000 characters.
> **Score:** 20 pts. (a) New knowledge/tech + (b) Enhanced vs SOTA + (c) Future potential. All three = 20; two = 15; one = 5.

---

## Draft (workspace markdown — strip headings before submission)

**(a) New knowledge / technology.**

(1) *Self-supervised cross-modal re-identification — the core contribution.* A learned model uses **AIS-on co-occurrence as a free training signal**: when a vessel broadcasts AIS, its identity labels the concurrent SAR/EO/acoustic/RF/text observations in its space-time neighborhood, letting the model learn the cross-modal association [I1, I2, I6]; at inference it re-identifies vessels whose AIS is **dark**. Self-supervised learned dark-vessel re-ID across this modality set is, to the applicant's knowledge, absent from the literature — and it dissolves the scarce-label barrier that blocks supervised maritime fusion. It is novel, separately patentable foreground IP the applicant will protect — going well beyond the prior, named-inventor entity-resolution patents.

(2) *Uncertainty-propagating, calibrated fusion.* Per-modality evidential encoders [I5] feed a transformer association head over the set of concurrent observations [I3]; the fused identity score is conformally calibrated [E1, E2] for distribution-free coverage — uncertainty is a first-class, propagated output, not a post-hoc number. This is the *"advanced deep learning for spatiotemporal alignment, uncertainty propagation, and confidence scoring"* CH13 asks for.

(3) *Adaptive, constraint-aware perception.* An agentic controller rewrites the edge perception graph (MediaPipe, text-defined `.pbtxt` [H1]) to swap networks under SWaP / threat conditions — a concrete realization of CH13's *"learned, adaptive fusion across modalities rather than static aggregation."*

**(b) Enhanced capability vs SOTA.** *Learned vs aggregated:* a trained cross-modal association model replaces rule-based / threshold fusion — the exact distinction CH13 draws ("learned, adaptive" vs "static aggregation"). *Explainability:* attention weights over modalities plus a per-output causation chain make every identity decision inspectable in one application, accreditable as a system property, beyond post-hoc XAI. *Human-in-the-loop:* analyst adjudication is a first-class signal that closes an active-learning loop — the applicant's shipped collaborative-intelligence pattern from the Orchive [A6a][A9], now applied to ISR decision-making and speed. *Methodological pedigree:* semi-supervised deep learning at archive scale on unlabeled acoustic streams (ORCA-SLANG [A2]; Sattar 2011, 95 % on ONC data [A1]) is the applicant's direct, peer-reviewed evidence the learning approach transfers to scarce-label maritime fusion.

**(c) Future potential.** The "intermittent strong identifier self-supervises the rest" pattern generalizes to any multi-domain setting with a sometimes-present anchor — Arctic ISR (sat + RF + telemetry), Airborne Multi-Sensor (radar + EO/IR + telemetry), land / edge tactical (audio + video + sensor) — without redesign. The calibrated fusion head, the provenance-tracked entity graph, and the human-in-the-loop layer carry forward to Component 1b's second domain and to allied-interoperable maritime domain awareness.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars. Tighten (a)(2) and (c) if over.

## Cross-references to workspace

- Composition novelty: `02-STRATEGY.md` "What's actually built today" + `03-ARCHITECTURE.md`.
- Prior patents (named-inventor background) + new patentable IP framing: `04-PORTFOLIO.md` Tier 1B + `06-REFERENCES.md` [A8a, A8b].
- doibio lift: `03-ARCHITECTURE.md` Layer 4.
- Calibration: `06-REFERENCES.md` [E1][E2].
