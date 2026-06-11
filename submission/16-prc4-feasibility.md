# Field 16 — PRC-4: Feasibility and Approach

**Form section:** Component 1a → PRC-4
**Cap:** 3000 characters
**LOCAL CHAR COUNT:** 2567 (cap 3000) — planetar.ca in M6; internal paths removed
**Source:** `proposal/PRC-4_feasibility.md` — RE-CENTERED 2026-05-30 (learned-fusion-model thesis); blank lines stripped per DIP rule.
**Status:** ✅ READY (re-centered)

## Paste-protocol notes

- Blank lines stripped (they count toward the DIP cap). Single newline between paragraphs.
- Markdown emphasis removed; (a)/(b)/(c) and (i)/(ii) labels kept as plain text.
- After paste, DIP's counter should read ~2567 (±2 for newline encoding). (c) risks are now inline-labelled prose (no bullet markers) so the field renders as paragraphs, not a run-on dash list.

--- PASTE THIS BELOW ---
(a) Achievable in practice. The substrate that makes the model demonstrable is already built and open-source (~13 k LOC of Planetar services): a provenance-tracked bus, a typed envelope, an entity-graph service (planetar-ontology, 30 tests; successor to the ~18-month doibio), an analyst shell, and four broker-integrated per-modality services (AIS, Sentinel-1 SAR, EO, hydrophone) that become the model's encoders. The 1a builds the new science on top: the self-supervised cross-modal fusion model, plus text and RF encoders.
Public-data coverage: AIS (MarineCadastre / GFW) supplies both signal and the self-supervision labels; SAR (Sentinel-1, xView3); EO (Singapore Maritime / MODS); acoustic (ONC, ShipsEar, DeepShip); text (notices-to-mariners, port-state-control, OSINT — public); RF (best-effort public / synthetic — see risks). No GFP, no classified data. Compute fits ~$18 K cloud — fine-tuning public encoders, not frontier-scale training.
Solo execution is realistic: the applicant authored the substrate and has shipped comparable systems and peer-reviewed ML.
(b) Well-reasoned approach. Two milestone stages, each with measurable exit criteria.
Milestone 1: harden the substrate and the four ingresses and scaffold text + RF; build per-modality encoders into a shared embedding and the AIS-on co-occurrence training set; train the self-supervised cross-modal association model (the primary R&D).
Milestone 2: propagate evidential uncertainty with conformal coverage and evaluate AIS-off re-identification; retype the vessel-entity graph; build the explainable, human-in-the-loop analyst surface (attention and causal-chain views, analyst adjudication); deliver the end-to-end Salish-Sea demonstration deployed live at planetar.ca for the evaluator to operate, with public-benchmark evaluation and the 1b package.
The model fine-tunes public baselines, not frontier-scale training.
(c) Risks + mitigations. RF data scarce publicly (medium-high): the RF encoder is built and evaluated on best-effort public / synthetic data; if unavailable, fall back to the five-modality model (AIS / SAR / EO / acoustic / text) — still well past the two-type bar, no overclaim. Self-supervision signal noisy (medium): supervised fine-tune on xView3 as an anchor; conformal coverage holds even if the score is weak. Acoustic vessel-transit GT sparse (medium): semi-supervised per ORCA-SLANG, discrimination via ShipsEar / DeepShip. Founder capacity (medium): a built-in schedule buffer is scope-cut insurance — cut RF or the waveform view before extending the timeline.
--- END PASTE ---
