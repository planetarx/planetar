# PRC-4 — Feasibility & Approach

> **Field cap:** 3,000 characters.
> **Score:** 20 pts. (a) Achievable + (b) Well-reasoned + (c) Risks + mitigations. All three = 20.

---

## Draft (workspace markdown — strip headings before submission)

**(a) Achievable in practice.** The substrate that makes the model demonstrable is already built and open-source (~13 k LOC `planetar-*`; `docs/built-services-inventory.md`): a provenance-tracked bus, a typed envelope, an entity-graph service (`planetar-ontology`, 30 tests; successor to the ~18-month `doibio`), an analyst shell, and four broker-integrated per-modality services (AIS, Sentinel-1 SAR, EO, hydrophone) that become the model's encoders. The 1a builds the new science on top: the self-supervised cross-modal fusion model, plus text and RF encoders.

Public-data coverage (`05-DATASETS.md`): AIS (MarineCadastre / GFW) supplies both signal and the self-supervision labels; SAR (Sentinel-1, xView3 [D1]); EO (Singapore Maritime / MODS); acoustic (ONC, ShipsEar [D4], DeepShip [D5]); text (notices-to-mariners, port-state-control, OSINT — public); RF (best-effort public / synthetic — see risks). No GFP, no classified data. Compute fits ~$18 K cloud — fine-tuning public encoders, not frontier-scale training.

Solo execution is realistic: the applicant authored the substrate and has shipped comparable systems and peer-reviewed ML.

**(b) Well-reasoned approach.** Six milestones (`07-TIMELINE.md`):

- *M1 — Substrate + ingest:* harden the bus and four ingresses; scaffold text + RF ingest.
- *M2 — Encoders + pairs:* per-modality encoders into a shared embedding; build the AIS-on co-occurrence training set.
- *M3 — Fusion model (primary R&D):* self-supervised cross-modal association trained on AIS co-occurrence.
- *M4 — Calibration + dark-vessel eval:* evidential uncertainty + conformal coverage; AIS-off re-ID evaluation; vessel-entity retype.
- *M5 — Explainable + human-in-the-loop surface:* attention and causal-chain views; analyst adjudication loop.
- *M6 — Demo + report:* end-to-end Salish-Sea dark-event scenario, deployed live at planetar.ca for evaluators to operate; public-benchmark eval; 1b package.

Each milestone has measurable exit criteria; the model fine-tunes public baselines, not frontier training.

**(c) Risks + mitigations.** *RF data scarce publicly (medium-high):* the RF encoder is built and evaluated on best-effort public / synthetic data; if unavailable, fall back to the five-modality model (AIS / SAR / EO / acoustic / text) — still well past the two-type bar, no overclaim. *Self-supervision signal noisy (medium):* supervised fine-tune on xView3 [D1] as an anchor; conformal coverage holds even if the score is weak. *Acoustic vessel-transit GT sparse (medium):* semi-supervised per ORCA-SLANG [A2], discrimination via ShipsEar / DeepShip [D4, D5]. *Founder capacity (medium):* the M6 buffer is scope-cut insurance — cut RF or the waveform view before extending the timeline.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars.

## Cross-references

- Detailed M1–M6: `07-TIMELINE.md` execution calendar.
- Risk register: `07-TIMELINE.md` end of file.
- Code already-existing: `04-PORTFOLIO.md` "Working code" + `02-STRATEGY.md` "What's actually built today."
- Datasets: `05-DATASETS.md`.
