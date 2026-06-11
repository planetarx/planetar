# Milestone entry guide — every field, both milestones

**DIP page:** Work Plan and Deliverables → Milestone 1 / Milestone 2
**Budget:** $157,924. Milestone 1 = $80,024 (**50.7%** ≤70% ✅). Milestone 2 = $77,900.
**Each activity has 5 sub-fields** (Activity / Deliverable / Level of Effort / Risk / Mitigation), each ≤800 chars. No blank lines inside any box.
**Level of Effort per milestone sums to 13 weeks.**

═══════════════════════════════════════
# MILESTONE 1

**Total Performance Period (in weeks):** `13`

---
## Activity 1 — "Add Activity"

**Activity:**
```
Harden the real-time data substrate — the message bus, the typed provenance-carrying envelope, and write-ahead-log replay — and place it under continuous integration with regression tests so replay is bit-for-bit reproducible. This is the transport layer every modality publishes onto, fixed before any ingest or model work depends on it.
```
**Description of Deliverable:**
```
A hardened bus + typed-envelope + WAL-replay substrate under continuous integration, with regression gates passing and replay reproducible bit-for-bit. The shared transport every ingest service publishes onto.
```
**Estimated Level of Effort (Weeks):** `1`

**Risk(s): Description, probability and impact:**
```
The substrate is already built, so risk is low; hardening could expose latent replay edge cases (low probability). Impact: minor rework contained within this activity, no effect on the downstream schedule.
```
**Risk mitigation strategy(ies):**
```
Lock the substrate behind regression tests before the ingest and model work depends on it, so any replay defect surfaces at the transport layer rather than downstream in training.
```

---
## Activity 2 — "Add Activity"

**Activity:**
```
Harden the four existing per-modality ingest services (AIS, Sentinel-1 SAR, electro-optical, hydrophone) and add two new ingest paths — text (notices-to-mariners, port-state-control reports, open-source maritime intelligence) and radio-frequency emissions — so every input arrives as a typed, time-stamped, provenance-tagged observation on public data, under continuous integration.
```
**Description of Deliverable:**
```
All six modalities (AIS, SAR, EO, acoustic, RF, text) streaming typed, provenance-tagged observations from public sources onto the bus, with regression gates passing. This is the input layer the fusion model consumes.
```
**Estimated Level of Effort (Weeks):** `2`

**Risk(s): Description, probability and impact:**
```
RF is the hardest modality to source publicly (medium-high probability). Impact: the model fuses five modalities instead of six, slightly narrowing coverage — but it still exceeds the required two heterogeneous types, so impact on the core objective is low.
```
**Risk mitigation strategy(ies):**
```
Source RF from best-effort public and synthetic data. If public RF proves insufficient, proceed with the five-modality model (AIS, SAR, EO, acoustic, text) — no overclaim, still well past the essential two-type bar. Scope the RF ingest path so it can be dropped without affecting the other five.
```

---
## Activity 3 — "Add Activity"

**Activity:**
```
Build per-modality encoders that project each observation — SAR chip, EO chip, acoustic spectrogram, AIS kinematics, RF signature, and report text — into one shared embedding space, fine-tuning established public baselines (CLIP, I-JEPA, triplet-loss re-identification families) rather than training at frontier scale.
```
**Description of Deliverable:**
```
A set of per-modality encoders producing comparable embeddings in one shared space — the representation layer the fusion model operates over.
```
**Estimated Level of Effort (Weeks):** `2`

**Risk(s): Description, probability and impact:**
```
Modalities with scarce public labels (RF, text) may yield weak embeddings (medium probability). Impact: lower-quality embeddings for those modalities, partly recovered by the co-occurrence training in Activity 5.
```
**Risk mitigation strategy(ies):**
```
Fine-tune established public baselines rather than training from scratch; the applicant's peer-reviewed semi-supervised deep learning on hydrophone archives is precedent that the approach transfers to scarce-label modalities.
```

---
## Activity 4 — "Add Activity"

**Activity:**
```
Construct the self-supervision training set from AIS-on periods: when a vessel is broadcasting AIS, its identity automatically labels the concurrent observations from the other modalities in the same place and time, producing cross-modal training pairs at no labelling cost. Anchor with the publicly labelled xView3 SAR/AIS dark-vessel dataset and quantify label quality before training.
```
**Description of Deliverable:**
```
A labelled cross-modal co-occurrence dataset built from public AIS-on periods, with measured co-occurrence-pairing precision — the training data for the fusion model.
```
**Estimated Level of Effort (Weeks):** `2`

**Risk(s): Description, probability and impact:**
```
The self-supervision signal is noisy where AIS coverage is sparse or intermittent (medium probability). Impact: weaker training pairs could reduce the accuracy of the learned cross-modal association.
```
**Risk mitigation strategy(ies):**
```
Anchor the training set with the publicly labelled xView3 SAR/AIS dark-vessel dataset, and quantify label quality (precision of the co-occurrence pairing) before training so noisy pairs are filtered or down-weighted.
```

---
## Activity 5 — "Add Activity"

**Activity:**
```
Train the self-supervised cross-modal fusion model — a transformer association head operating over the set of concurrent observation embeddings — to learn vessel identity from the co-occurrence signal.
```
**Description of Deliverable:**
```
A trained cross-modal fusion model that associates concurrent observations to a common vessel identity, validated on held-out public data.
```
**Estimated Level of Effort (Weeks):** `4`

**Risk(s): Description, probability and impact:**
```
The model may under-perform on modalities with scarce labels (medium probability). Impact: lower association accuracy for those modalities, though calibration in Milestone 2 bounds the resulting uncertainty.
```
**Risk mitigation strategy(ies):**
```
Build on the fine-tuned public baselines from Activity 3 rather than training at frontier scale; the applicant's peer-reviewed semi-supervised deep learning on hydrophone archives is direct precedent that the approach transfers to the scarce-label regime.
```

---
## Activity 6 — "Add Activity"

**Activity:**
```
Apply the trained fusion model to re-identify vessels whose AIS has gone dark, producing cross-modal re-identification candidates, each carrying its per-modality supporting evidence; demonstrate end-to-end on held-out public data.
```
**Description of Deliverable:**
```
Dark-vessel re-identification candidates with per-modality supporting evidence, demonstrated end-to-end on held-out public data — the Milestone 1 capability proof.
```
**Estimated Level of Effort (Weeks):** `2`

**Risk(s): Description, probability and impact:**
```
Re-identification precision on dark vessels may be modest before calibration (medium probability). Impact: candidate confidence is not yet bounded — addressed directly by the conformal calibration in Milestone 2.
```
**Risk mitigation strategy(ies):**
```
Treat Milestone 1 output as ranked candidates with evidence, not hard decisions; Milestone 2 adds distribution-free confidence bounds before any operational claim.
```

---
## Milestone 1 — Financials
- **Labour:** Proposer-scientist (PhD CS/ML), **440 hrs × $140/hr = $61,600**
- **Materials:** MacBook Pro (local AI training + development) ×1 @ $7,924 = **$7,924** · **Travel:** $0
- **Other Costs:** Cloud/GPU `9,000` · Software `1,000` · Datasets `500` → **$10,500**
- **TOTAL FIRM MILESTONE 1 PRICE: $80,024** (50.7% ≤ 70% ✅)

═══════════════════════════════════════
# MILESTONE 2

**Total Performance Period (in weeks):** `13`

---
## Activity 1 — "Add Activity"

**Activity:**
```
Propagate per-modality uncertainty through the fusion model: per-modality evidential heads are carried through the transformer association head so that every fused re-identification score arrives with a principled uncertainty estimate rather than as a bare point score. This makes model confidence a first-class, propagated output, ready for calibration.
```
**Description of Deliverable:**
```
A fusion model that emits, for each re-identification, a fused score together with a propagated per-modality uncertainty estimate.
```
**Estimated Level of Effort (Weeks):** `2`

**Risk(s): Description, probability and impact:**
```
Evidential heads may be poorly calibrated on scarce-label modalities such as RF and text (medium probability). Impact: the raw uncertainty estimates could be over- or under-confident before calibration.
```
**Risk mitigation strategy(ies):**
```
Feed the propagated uncertainty into the conformal calibration step (Activity 2), which guarantees coverage regardless of the raw estimate — a distribution-free back-stop, so confidence is sound after calibration even if the raw heads are imperfect.
```

---
## Activity 2 — "Add Activity"

**Activity:**
```
Calibrate the fused re-identification score with conformal prediction, converting it into a distribution-free confidence bound so each re-identification carries a coverage-guaranteed confidence interval instead of an uncalibrated number. Validate the empirical coverage on held-out public data.
```
**Description of Deliverable:**
```
A calibrated fusion model whose re-identification outputs carry distribution-free confidence intervals at a stated, empirically validated coverage level.
```
**Estimated Level of Effort (Weeks):** `1`

**Risk(s): Description, probability and impact:**
```
Calibration could be weak if the underlying re-identification scores are noisy (low probability). Impact: confidence bounds would be wider than ideal, but they remain valid.
```
**Risk mitigation strategy(ies):**
```
Conformal prediction guarantees the stated coverage even when the base score is weak — a distribution-free back-stop, so calibrated confidence is assured regardless of score quality.
```

---
## Activity 3 — "Add Activity"

**Activity:**
```
Evaluate AIS-off (dark-vessel) re-identification on public benchmarks, producing an evaluation report stratified by vessel class and geographic region so performance is measured rather than asserted, with calibrated confidence reported on each result.
```
**Description of Deliverable:**
```
A public-benchmark evaluation report on dark-vessel re-identification, stratified by vessel class and geographic region, with calibrated confidence on each result.
```
**Estimated Level of Effort (Weeks):** `1`

**Risk(s): Description, probability and impact:**
```
Public dark-vessel ground truth is uneven across vessel classes and regions (medium probability). Impact: thin strata yield wider confidence intervals and weaker per-class conclusions.
```
**Risk mitigation strategy(ies):**
```
Anchor on the publicly labelled xView3 SAR/AIS dark-vessel dataset and report per-stratum support, so thin strata are flagged rather than over-claimed.
```

---
## Activity 4 — "Add Activity"

**Activity:**
```
Retype the vessel-entity graph for the maritime domain so each resolved identity carries full provenance — which observation, from which sensor, at what time, supported the match — making every re-identification traceable to its raw inputs. This builds on the applicant's existing ~18-month entity-resolution implementation.
```
**Description of Deliverable:**
```
A vessel-domain entity graph in which each canonical identity links to its supporting observations with full per-match provenance.
```
**Estimated Level of Effort (Weeks):** `1`

**Risk(s): Description, probability and impact:**
```
Retyping the entity model to the vessel domain may surface schema mismatches with the existing implementation (low probability). Impact: minor rework of the type mappings, contained within this activity.
```
**Risk mitigation strategy(ies):**
```
Reuse the ~18-month entity-resolution implementation as the starting point and retype incrementally, validating each mapping against the bus envelope schemas before moving on.
```

---
## Activity 5 — "Add Activity"

**Activity:**
```
Build the explainable, human-in-the-loop analyst surface on the existing shell: attention-weighted evidence views showing which modalities and observations drove each re-identification, and click-through from any output back to its raw inputs, so an operator can adjudicate each candidate. Capture the operator's accept/reject/correct decisions as labelled feedback for future model refinement.
```
**Description of Deliverable:**
```
An operator console driving the live model, with attention-based explanations, click-through to raw evidence, and adjudication capture that feeds an active-learning loop.
```
**Estimated Level of Effort (Weeks):** `4`

**Risk(s): Description, probability and impact:**
```
Viewer scope could overrun the schedule (medium probability). Impact: less time remaining for the final integrated demonstration.
```
**Risk mitigation strategy(ies):**
```
Ship the map, timeline and entity-card views first; the raw-evidence (waveform/chip) view ships as a minimal renderer. Non-essential viewer scope is cut before the schedule slips.
```

---
## Activity 6 — "Add Activity"

**Activity:**
```
Integrate the full pipeline into an end-to-end demonstration: a synthetic Salish Sea scenario in which a vessel disables AIS during a Sentinel-1 satellite pass and is re-identified across SAR, EO and acoustic, replayable bit-for-bit. Deploy a live system at planetar.ca that the Technical Authority can operate during evaluation. Run a public-benchmark evaluation and prepare the Component 1b proposal.
```
**Description of Deliverable:**
```
A live demonstrator at planetar.ca the evaluator can operate, a replayable end-to-end Salish Sea dark-vessel scenario, a public-benchmark evaluation report, and the Component 1b proposal package.
```
**Estimated Level of Effort (Weeks):** `4`

**Risk(s): Description, probability and impact:**
```
Solo-founder capacity is the main schedule risk (medium probability). Impact: feature scope at risk near the deadline.
```
**Risk mitigation strategy(ies):**
```
A built-in schedule buffer; non-essential scope (the RF modality, the raw-evidence viewer) is cut before the timeline extends. The substrate is already built, so effort concentrates on the model and demo, not plumbing.
```

---
## Milestone 2 — Financials
- **Labour:** Proposer-scientist, **485 hrs × $140/hr = $67,900**
- **Materials:** $0 · **Travel:** $0
- **Other Costs:** Cloud/GPU `9,000` · Software `500` · Datasets `500` → **$10,000**
- **TOTAL FIRM MILESTONE 2 PRICE: $77,900**

═══════════════════════════════════════
**Grand total: $157,924** · M1 50.7% + M2 49.3% · all CAD, excl. taxes.
