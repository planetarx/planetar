# Fields 20–22 — Work Plan & Deliverables (2 milestones) + Total — THE ACTUAL FINANCIAL ENTRY

**Form section:** Component 1a → Work Plan and Deliverables → Milestone 1 / Milestone 2 / Total Firm Milestone Price
**Status:** ✅ $157,924 budget (2026-05-30 filed bid). This 2-milestone form is where SC-1 + PRC-7 are assessed — it is the real financial proposal (the old 6-row `19-prc7` is now just an internal cost summary).
**Key rule:** Milestone 1 ≤ 70% of total. Ours = **50.7%** ✅. Total = **$157,924**.
**Rate:** **$140/hr fully-loaded** (includes G&A/overhead — so there is no separate overhead line; avoids the §3.7 eligibility question). 925 hrs total.
**Per-entry cap:** Work-activity entries ≤ **800 characters** each (all verified under). No blank lines.

| Internal | → Form stage | Cost | % |
|---|---|---|---|
| M1+M2+M3 (substrate+ingest / encoders / train model) | **Milestone 1** | $80,024 | 50.7% |
| M4+M5+M6 (calibrate+eval / explainable surface / demo+planetar.ca) | **Milestone 2** | $77,900 | 49.3% |

---

## MILESTONE 1

**Total Performance Period (in weeks):** `13`

**Work Activities & Risks and Mitigation** — add six activity rows:

--- ACTIVITY 1 ---
Activity: Harden the real-time data substrate (message bus, typed envelope, write-ahead-log replay) and place it under continuous integration so replay is bit-for-bit reproducible. Deliverable: a hardened bus + envelope + WAL-replay substrate under CI — the transport every modality publishes onto. Risk: substrate already built, hardening may expose latent replay edge cases (low). Mitigation: lock it behind regression tests before the ingest and model work depends on it.
--- ACTIVITY 2 ---
Activity: Harden the four existing per-modality ingest services (AIS, Sentinel-1 SAR, electro-optical, hydrophone) and add two new ingest paths — text (notices-to-mariners / port-state / OSINT) and RF emissions — so every input arrives as a typed, time-stamped, provenance-tagged observation on public data, under CI. Deliverable: all six modalities streaming typed, provenance-tagged observations on public data. Risk: RF is hard to source publicly (medium-high). Mitigation: source RF from best-effort public/synthetic data; if insufficient, proceed with a five-modality model (AIS/SAR/EO/acoustic/text) — still well past the two-type requirement, no overclaim.
--- ACTIVITY 3 ---
Activity: Build per-modality encoders that project each observation (SAR chip, EO chip, acoustic spectrogram, AIS kinematics, RF signature, report text) into one shared embedding space, fine-tuning established public baselines (CLIP / I-JEPA / triplet-loss families) rather than frontier-scale training. Deliverable: per-modality encoders producing comparable embeddings in one shared space — the representation layer the fusion model operates over. Risk: scarce-label modalities (RF, text) may yield weak embeddings (medium). Mitigation: fine-tune public baselines; the applicant's peer-reviewed semi-supervised acoustic ML is precedent it transfers.
--- ACTIVITY 4 ---
Activity: Construct the self-supervision training set from AIS-on periods — a broadcasting vessel's identity labels its concurrent cross-modal observations in the same place and time, producing training pairs at no labelling cost; anchor with the publicly labelled xView3 SAR/AIS dark-vessel dataset and quantify label quality before training. Deliverable: a labelled cross-modal co-occurrence dataset on public data with measured pairing precision. Risk: the self-supervision signal is noisy where AIS is sparse (medium). Mitigation: anchor with xView3 and quantify co-occurrence-pairing precision so noisy pairs are filtered or down-weighted.
--- ACTIVITY 5 ---
Activity: Train the self-supervised cross-modal fusion model — a transformer association head over the set of concurrent observation embeddings — to learn vessel identity from the co-occurrence signal. Deliverable: a trained fusion model that associates concurrent observations to a common vessel identity, validated on held-out public data. Risk: under-performance on scarce-label modalities (medium). Mitigation: build on the fine-tuned public baselines, not frontier-scale training; the applicant's peer-reviewed semi-supervised acoustic ML is direct precedent.
--- ACTIVITY 6 ---
Activity: Apply the trained model to re-identify vessels whose AIS is dark, producing cross-modal re-identification candidates each carrying per-modality supporting evidence; demonstrate end-to-end on held-out public data. Deliverable: dark-vessel re-identification candidates with per-modality evidence, demonstrated end-to-end on public data — the Milestone 1 capability proof. Risk: re-identification precision on dark vessels may be modest before calibration (medium). Mitigation: treat output as ranked candidates with evidence, not hard decisions; Milestone 2 adds distribution-free confidence bounds.
--- END ACTIVITIES ---

**Financials — Milestone 1**

- **Labour** (Add Labour): Proposer-scientist (PhD CS/ML), fully-loaded — **440 hrs × $140/hr = $61,600**. → Total Labour: **$61,600**
- **Materials** (Add Materials): MacBook Pro (local AI training + development), 1 × $7,924 → **$7,924**
- **Travel**: none → **$0**
- **Other Costs** (Add Other Costs):
  - Cloud / GPU compute — **$9,000**
  - Software / tools — **$1,000**
  - Datasets / licences — **$500**
  - → Total Other Costs: **$10,500**
- **TOTAL FIRM MILESTONE 1 PRICE: $80,024**  (50.7% of total — ≤70% ✅)

---

## MILESTONE 2

**Total Performance Period (in weeks):** `13`

**Work Activities & Risks and Mitigation** — add six activity rows:

--- ACTIVITY 1 ---
Activity: Propagate per-modality uncertainty through the fusion model — per-modality evidential heads carried through the transformer association head so each fused re-identification score arrives with a principled uncertainty estimate, not a bare point score. Deliverable: a fusion model that emits, per re-identification, a fused score plus a propagated per-modality uncertainty estimate. Risk: evidential heads may be poorly calibrated on scarce-label modalities (medium). Mitigation: the conformal step (Activity 2) guarantees coverage regardless of the raw estimate — a distribution-free back-stop.
--- ACTIVITY 2 ---
Activity: Calibrate the fused re-identification score with conformal prediction, turning it into a distribution-free confidence bound so each re-identification carries a coverage-guaranteed interval; validate coverage on held-out public data. Deliverable: a calibrated model whose outputs carry distribution-free confidence intervals at a stated, validated coverage level. Risk: calibration weak if scores are noisy (low). Mitigation: conformal prediction guarantees the stated coverage even when the base score is weak — a built-in back-stop.
--- ACTIVITY 3 ---
Activity: Evaluate AIS-off (dark-vessel) re-identification on public benchmarks, producing an evaluation report stratified by vessel class and geographic region with calibrated confidence on each result. Deliverable: a public-benchmark evaluation report on dark-vessel re-identification, stratified by vessel class and region. Risk: public dark-vessel ground truth is uneven across classes / regions (medium). Mitigation: anchor on the publicly labelled xView3 SAR/AIS dataset and report per-stratum support so thin strata are flagged, not over-claimed.
--- ACTIVITY 4 ---
Activity: Retype the vessel-entity graph for the maritime domain so each resolved identity carries full provenance — which observation, from which sensor, at what time, supported the match — building on the applicant's ~18-month entity-resolution implementation. Deliverable: a vessel-domain entity graph linking each canonical identity to its supporting observations with full per-match provenance. Risk: retyping may surface schema mismatches with the existing implementation (low). Mitigation: retype incrementally from the existing implementation, validating each mapping against the bus envelope schemas.
--- ACTIVITY 5 ---
Activity: Build the explainable, human-in-the-loop analyst surface on the existing shell — attention-weighted evidence views showing which modalities and observations drove each re-identification, and click-through from any output to its raw inputs — so an operator can adjudicate each candidate; capture accept/reject/correct decisions as labelled feedback that closes an active-learning loop. Deliverable: an operator console driving the live model, with attention-based explanations, click-through to raw evidence, and adjudication capture feeding an active-learning loop. Risk: viewer scope overruns (medium). Mitigation: ship map + timeline + entity-card first; the raw-evidence view ships as a minimal renderer; scope is cut before the schedule slips.
--- ACTIVITY 6 ---
Activity: Integrate the full pipeline into an end-to-end demonstration — a synthetic Salish Sea scenario where a vessel disables AIS during a Sentinel-1 pass and is re-identified across SAR/EO/acoustic, replayable bit-for-bit; deploy a live system at planetar.ca that the Technical Authority can operate during evaluation; run a public-benchmark evaluation and prepare the Component 1b proposal. Deliverable: a live demonstrator at planetar.ca, a replayable Salish Sea scenario, an evaluation report, and the 1b package. Risk: solo-founder capacity (medium). Mitigation: a built-in schedule buffer; non-essential scope (RF, raw-evidence view) is cut before the timeline extends.
--- END ACTIVITIES ---

**Financials — Milestone 2**

- **Labour**: Proposer-scientist, fully-loaded — **485 hrs × $140/hr = $67,900** → Total Labour: **$67,900**
- **Materials**: none → **$0**
- **Travel**: none → **$0**
- **Other Costs**:
  - Cloud / GPU compute — **$9,000**
  - Software / tools — **$500**
  - Datasets / licences — **$500**
  - → Total Other Costs: **$10,000**
- **TOTAL FIRM MILESTONE 2 PRICE: $77,900**

---

## TOTAL FIRM MILESTONE PRICE

- Milestone 1: **$80,024** (50.7%)
- Milestone 2: **$77,900** (49.3%)
- **TOTAL: $157,924**
- Milestone 1 share = **50.7%** ≤ 70% → **SC-1 passes**. Total $157,924 = 63% of the $250K contract cap.

**Reconciliation:** Labour $129,500 (925 hrs × $140 fully-loaded) + Materials $7,924 (MacBook Pro, M1) + Other Costs $20,500 (cloud $18,000 + software $1,500 + datasets $1,000) = **$157,924**. Travel $0. Hours: M1 440 + M2 485 = 925.

---

## Remaining wizard steps (prep on request)

- **Location and Language of Work** — Victoria, BC, Canada; English. Solo, remote/home-office.
- **Glossary** — define AIS, SAR, EO, RF, ISR, TRL, etc.
- **Reference Documents** — open-source repos + key citations (US Patents 10,936,582 & 11,442,952; PhD thesis; ORCA-SLANG; Sattar 2011; xView3).
- **Progression to Follow-on Component** — Component 1b (TRL 4–5): productionize, multi-domain (Arctic/airborne), CAF partner.
- **Certifications Required with the Bid** — standard PWGSC certs (Canadian-controlled, no conflict, etc.).
