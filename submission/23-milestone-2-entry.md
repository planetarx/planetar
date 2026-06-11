# Milestone 2 — exactly what to enter in each form field

**DIP page:** Work Plan and Deliverables → Milestone 2
**Milestone 2 total = $77,900** (M1 $80,024 + M2 $77,900 = $157,924).
**Each activity has 5 sub-fields** (Activity / Deliverable / Level of Effort / Risk / Mitigation), each ≤800 chars. No blank lines inside any box.
**Level of Effort sums to 13 weeks** (2 + 1 + 1 + 1 + 4 + 4).

---

**Total Performance Period (in weeks):** `13`

═══════════════════════════════════════
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

═══════════════════════════════════════
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

═══════════════════════════════════════
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

═══════════════════════════════════════
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

═══════════════════════════════════════
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

═══════════════════════════════════════
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

═══════════════════════════════════════
## Milestone 2 — Financials

### Labour — "Add Labour" (one row)
| Field | Value |
|---|---|
| Category | `Principal Investigator (PhD, Computer Science / Machine Learning)` |
| Labour (h) | `485` |
| Rate ($/h) | `140.00` |
| Total ($) | `67900.00` |

### Materials — skip → $0.00
### Travel — skip → $0.00

### Other Costs — "Add Other Costs" (three rows)
**Row 1**
- Other cost: `Cloud / GPU compute`
- Description: `Cloud GPU/CPU compute for model training, calibration and evaluation runs, the public-benchmark evaluation, and hosting the live planetar.ca demonstrator the evaluator can operate.`
- Cost ($): `9000.00`

**Row 2**
- Other cost: `Software / tools`
- Description: `Development and operations tooling: observability/monitoring, continuous-integration runner minutes, and ancillary paid developer services.`
- Cost ($): `500.00`

**Row 3**
- Other cost: `Datasets / licences`
- Description: `Reserve for paid dataset access if a public dataset proves insufficient; core training and evaluation data are public and free.`
- Cost ($): `500.00`

→ Total Other Costs: **$10,000**

### TOTAL FIRM MILESTONE 2 PRICE: **$77,900**  ($67,900 labour + $10,000 other)

═══════════════════════════════════════
**Cross-check:** M1 $80,024 + M2 $77,900 = **$157,924** · keep the Labour **Category** identical to Milestone 1.
