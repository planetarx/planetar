# Field 14 — PRC-2: Novel & Innovative Solution

**Form section:** Component 1a → PRC-2
**Cap:** 3000 characters
**LOCAL CHAR COUNT:** 2921 (cap 3000)
**Source:** `proposal/PRC-2_novelty.md` — RE-CENTERED 2026-05-30 (learned-fusion-model thesis); blank lines stripped per DIP rule.
**Status:** ✅ READY (re-centered)

## Paste-protocol notes

- Blank lines stripped (they count toward the DIP cap). Single newline between paragraphs.
- Markdown emphasis removed; (a)/(b)/(c) and (i)/(ii) labels kept as plain text.
- After paste, DIP's counter should read ~2921 (±2 for newline encoding); DIP's counter is authoritative — this is the tightest field (~79 chars headroom). Inline-labelled prose (no bullet markers).

--- PASTE THIS BELOW ---
(a) New knowledge / technology.
(1) Self-supervised cross-modal re-identification — the core contribution. A learned model uses AIS-on co-occurrence as a free training signal: when a vessel broadcasts AIS, its identity labels the concurrent SAR/EO/acoustic/RF/text observations in its space-time neighborhood, letting the model learn the cross-modal association; at inference it re-identifies vessels whose AIS is dark. Self-supervised learned dark-vessel re-ID across this modality set is, to the applicant's knowledge, absent from the literature — and it dissolves the scarce-label barrier that blocks supervised maritime fusion. It is novel, patentable foreground IP the applicant will protect — well beyond the prior patents.
(2) Uncertainty-propagating, calibrated fusion. Per-modality evidential encoders feed a transformer association head over the set of concurrent observations; the fused identity score is conformally calibrated for distribution-free coverage — uncertainty is a first-class, propagated output, not a post-hoc number. This is the "advanced deep learning for spatiotemporal alignment, uncertainty propagation, and confidence scoring" the Challenge asks for.
(3) Adaptive, constraint-aware perception. An agentic controller rewrites the edge perception graph (a text-defined MediaPipe .pbtxt graph) to swap networks under SWaP / threat conditions — a concrete realization of the Challenge's "learned, adaptive fusion across modalities rather than static aggregation."
(b) Enhanced capability vs SOTA. Learned vs aggregated: a trained cross-modal association model replaces rule-based / threshold fusion — the exact distinction the Challenge draws ("learned, adaptive" vs "static aggregation"). Explainability: attention weights over modalities plus a per-output causation chain make every identity decision inspectable in one application, accreditable as a system property, beyond post-hoc XAI. Human-in-the-loop: analyst adjudication is a first-class signal that closes an active-learning loop — the applicant's shipped collaborative-intelligence pattern from the Orchive, now applied to ISR decision-making and speed. Methodological pedigree: semi-supervised deep learning at archive scale on unlabeled acoustic streams (ORCA-SLANG; Sattar 2011, 95 % on ONC data) is the applicant's direct, peer-reviewed evidence the learning approach transfers to scarce-label maritime fusion.
(c) Future potential. The "intermittent strong identifier self-supervises the rest" pattern generalizes to any multi-domain setting with a sometimes-present anchor — Arctic ISR (sat + RF + telemetry), Airborne Multi-Sensor (radar + EO/IR + telemetry), land / edge tactical (audio + video + sensor) — without redesign. The calibrated fusion head, the provenance-tracked entity graph, and the human-in-the-loop layer carry forward to Component 1b's second domain and to allied-interoperable maritime domain awareness.
--- END PASTE ---
