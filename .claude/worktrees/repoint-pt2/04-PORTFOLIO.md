# 04 — Portfolio Audit: Citable Credentials

What's audit-defensible, what's working code, what's oral-history-only. Honesty bar: 6-year audit window post-award; everything claimed in the bid must survive that.

## Applicant

**Steven Randolph Ness, PhD**
- Company: **Zax Analytics** (incorporated in Canada).
- Canadian citizen.
- PhD: Computer Science, University of Victoria (2013). Thesis: *The Orchive*, supervised by George Tzanetakis (UVic CS, founder of Marsyas).
- Industry: Salesforce (ended 2021; produced two US patents). Currently Zax Analytics — AI for structural biology, scientific literature linking, and the planetar platform pitched here.
- Scholar metrics (per applicant): h-index 15, ~1,300 citations, i10-index 21.

---

## Tier 1 — Lead with these

### A. The maritime-acoustic anchor (carries the bid's ONC bond)

**Sattar, Driessen, Tzanetakis, Ness, Page (2011).** *"Automatic Event Detection for Long-Term Monitoring of Hydrophone Data."* IEEE PacRim 2011, pp. 668–674.

Why this is the strongest single credential in the portfolio:
- First author **Farook Sattar affiliated NEPTUNE Canada / UVic** at publication (NEPTUNE Canada = the operational name of Ocean Networks Canada's cabled observatory at the time).
- Evaluation on **NEPTUNE Canada operational hydrophone data**: Naxys Ethernet Hydrophone 02345, 96 kHz, 3000 m depth, ~5.5 GB/day.
- **95% accuracy / 94.3% F-measure** on whale-call event detection across 140 labeled events / 12 recordings.
- Acknowledgements (verbatim): *"The authors would like to gratefully acknowledge the support from Neptune Canada under CANARIE project."* This is the public, dated, peer-reviewed institutional bond between the applicant and Canada's national hydrophone infrastructure.
- Methodology — EMD + wavelet-packet + temporal-predictability event detection on continuous noisy maritime acoustic streams — is structurally the same template needed for **dark-vessel acoustic-cueing in CH13's Maritime Task Group example**. The transfer from "whale call event in noise" to "vessel transit event in noise" is methodologically clean.

This single reference simultaneously:
- Anchors the hydrophone modality.
- Anchors the Canadian institutional connection (CH13 reviewers will recognize NEPTUNE Canada / ONC immediately).
- Demonstrates working ML on continuous unlabeled marine acoustic streams.
- Pre-dates and is independent of any of the third-party foundation-model work the applicant has used since.

### B. Patent — entity-graph foundation

**US Patent 10,936,582 (granted 2021).** *"Integrated entity view across distributed systems."*

- Direct match to CH13 desired outcome #2 (entity resolution + dynamic knowledge graph).
- **Implemented in `~/data/dev/doibio`** (~18 months iteration, 20k+ LOC, 35 entity schemas, 630-line identity-resolution engine, event-sourced backend). Not theoretical IP — there is working code behind it. Lift details in `03-ARCHITECTURE.md` Layer 4.
- USPTO record is auditor-accessible via patent number.

### C. ORCA-SLANG (Interspeech 2021)

**Bergler, Schröter, Cheng, Barucija, Schmitt, Bardeli, Hofer, Symonds, Spong, Ness, Schneider, Maier (2021).** *"ORCA-SLANG: An Automatic Multi-Stage Semi-Supervised Deep Learning Framework for Large-Scale Killer Whale Call Type Identification."* Interspeech 2021.

- Direct methodological parallel to the planetar `acoustic.event` detector: massive unlabeled hydrophone archive → semi-supervised deep learning → multi-stage classification → calibrated outputs.
- Co-authors include **Andreas Maier (FAU Erlangen-Nürnberg)** and **Helena Symonds / Paul Spong (OrcaLab)** — international peer collaboration on operational maritime acoustic ML.
- The semi-supervised template applies cleanly to dark-vessel acoustic cueing where labels are scarce.

### D. Auditory Sparse Coding — Google co-authorship

**Ness, Walters, Lyon (2012).** *"Auditory Sparse Coding."* Chapter in *Music Data Mining* (Tao Li, Mitsunori Ogihara, George Tzanetakis, eds., CRC Press / Chapman & Hall).

- Co-authors **Thomas Walters and Richard F. Lyon** — both Google Research at the time. Lyon is the cochlea-modelling researcher behind CARFAC (the canonical biologically-inspired auditory front-end at Google).
- Establishes the applicant as a peer-level contributor on **biologically-inspired sparse representations for non-stationary acoustic signals** — exactly the representation class needed for hydrophone vessel-noise / biological-noise discrimination.
- Citable from PhD thesis Appendix B.2 (ref [160]).

---

## Tier 2 — Supporting credentials

### E. SOM-based unsupervised acoustic browsing

Two 2009 papers establishing the applicant's prior work on **unsupervised topology-preserving projection of acoustic features** — directly applicable to operator-facing visualization of unlabeled hydrophone embeddings (a TRL-3 deliverable for the entity-card / waveform viewers):

1. **Tzanetakis, Benning, Ness, Minifie, Livingston (2009).** *"Assistive music browsing using self-organizing maps."* PETRA 2009 (ACM 2nd Intl. Conf. on PErvasive Technologies Related to Assistive Environments).
2. **Ness, Tzanetakis (2009).** *"SOMba: Multiuser music creation using Self-Organizing Maps and Motion Tracking."* ICMC 2009.

Project audio feature vectors onto a 2D Kohonen map for navigable similarity grids. The mechanism is identical for a watch officer browsing acoustic similarity neighborhoods of unlabeled hydrophone contacts: SOMs work as well on vessel-noise as on music tags.

### F. Multi-output probabilistic classifier fusion

**Ness, Theocharis, Tzanetakis, Martins (2009).** *"Improving automatic music tag annotation using stacked generalization of probabilistic SVM outputs."* ACM Multimedia 2009. **139 citations.**

Multi-output probabilistic classification with ensemble fusion. Direct precedent for the multi-modal classification head pattern used in `vessel.v1.ReIDCandidate`'s log-score decomposition.

### G. The Orchive — large-scale archival ML

**Ness (2013).** *The Orchive: A System for Semi-Automatic Annotation and Analysis of a Large Collection of Bioacoustic Recordings.* PhD thesis, UVic. arXiv:1307.0589.

- The original semi-automatic-annotation system for the OrcaLab archive (decades of continuous hydrophone recordings off Hanson Island).
- Marsyas integration (the openmir codebase shells out to Marsyas binaries for feature extraction at production scale) — documented in code at `/home/sness/dev/orchive/openmir/`.
- Establishes long-term applicant engagement with **continuous-hydrophone ML at archive scale**, independent of any single paper.

### H. CRANK — automated scientific-computing pipelines

**Ness, McMullin, Pannu, Storoni, Liu, Cowtan, Read (2004).** *"CRANK: new methods for automated macromolecular crystal structure solution."* Structure 2004. **150 citations.**

Demonstrates ability to ship complex automated pipelines at scale — execution credibility for PRC-4 (feasibility). Off-domain, so not cited as methodological precedent, but useful as evidence of "this person delivers production-grade automated systems."

### I. Salesforce UI patent

**US Patent App 16/264,391 (2020).** *"User interface for commerce architecture."*

Secondary IP credential. Less directly relevant to CH13 than US 10,936,582 but shows industry IP track record.

---

## Tier 3 — Background (mention if narrative space allows)

- *A physical map of the mouse genome* (Nature 2002, 435 citations) — rigorous scientific computing at scale.
- *Structure-based design of TEM-1 β-lactamase inhibitors* (Biochemistry 2000, 180 citations).
- Monte Carlo docking algorithms (1995–1997).
- *Phase refinement through density modification* (2007).

These establish depth; they're not on the methodological critical path for planetar.

---

## Working code that is the applicant's own

Verified by git remote URL (`github.com/sness23/...`) and commit authorship — these are NOT third-party forks:

| Project | Stack | Relevance to planetar |
|---|---|---|
| **zbroker0** | C | Nanosecond bus. **Reproduced p50 = 80–140 ns / p99 = 400–900 ns over 1M-message benchmarks, 2026-04-27** (`docs/benchmark-2026-04-27.md`). Canonical spine. |
| **zmesg** | C / Go | Binary envelope (UUIDv7, ns timestamps, topic, correlation/causation). Canonical wire format. |
| **sales4** | TypeScript / React 19 / Node | Slack/Discord-style multi-client chat with µs-precision latency instrumentation. Becomes `planetar-ui`. |
| **doibio** | TypeScript / Node / SQLite | The patent's working POC. Event-sourced; 35 schemas; 630-line identity-resolution engine. Becomes `planetar-graph`. |
| **crank3** | Python | Evidence aggregation for competing hypotheses; canonical JSON schema; log-score decomposition (prior + evidence + uncertainty + penalty); provenance. Direct analog for cross-modal vessel re-ID. |
| **bioSkills** | Python / R | 365+ reusable ML workflows across bioinformatics — modular pipeline design credibility. |

---

## Power-user, not authorship — third-party foundation-model work

These are forks/clones of others' code that the applicant has used at production scale at Zax Analytics. Cited as evidence of **current** working familiarity with multi-modal foundation models, not as authored implementations:

- **Aeron** (LMAX-lineage messaging) — fork studied for the disruptor pattern.
- **Protenix** (ByteDance) — used as a multi-modal biomolecular foundation model.
- **chai-lab** (Chai Discovery), **boltz** (Wohlwend et al.), **chemeleon** (Burns), **DynamicBind** (Lu et al.) — all used as power user.

In the bid these become **one paragraph** in PRC-1: "the applicant is an active practitioner of state-of-the-art multi-modal foundation models, with working familiarity with their featurization, uncertainty heads, and cross-modality fusion patterns — methodological experience that transfers to the proposed maritime fusion application." No claim of authorship.

---

## ONC institutional history — what the bid can and can't say

**What the bid CAN claim (audit-defensible):**
- Co-authorship on Sattar et al. 2011 IEEE PacRim — 95% accuracy on **NEPTUNE Canada / ONC** hydrophone data with explicit Neptune Canada / CANARIE acknowledgement (above, Tier 1A).
- Production-scale Marsyas integration in the openmir codebase that runs the Orchive — documented in code.
- Co-authorship on ORCA-SLANG (Interspeech 2021) — semi-supervised hydrophone-archive ML at scale, peer-reviewed.

**What the bid CANNOT claim without further evidence:**
- "Paid research-assistant work for ONC" — not in PhD thesis acknowledgements (which credit NSERC fellowship + parents only); no contract, no thank-you, no public artifact found locally. *If the applicant locates a contract / email / paystub / public mention, this can be added.*
- "Built a custom Orchive for Richard Dewey at ONC" — no "Dewey" mention in any code, README, or commit log under `/home/sness`. Same caveat.

The proposal will lead with the co-authorship and the Marsyas-in-Orchive code. The two unverified claims do not appear in the narrative as facts. If the applicant wants to mention them, the bid will use language like "applicant has prior collaborative engagement with ONC researchers" — vague enough to be true without overclaiming, weak enough that nothing in the bid hangs on it.

---

## TRL-3 evidence — what the proposal points to

For MC-1 the bid claims **TRL 3–4 for the bus + envelope + shell** and **TRL 2 for cross-modal dark-vessel fusion at start, advancing to TRL 3 over the 1a**. Evidence on the table:

- ✅ **80–140 ns p50 SHM bus latency** — measured today, reproducible. (Bus / TRL 3+.)
- ✅ **Working multi-client microsecond-instrumented chat shell** — sales4. (Shell / TRL 3.)
- ✅ **Working entity-resolution engine + event-sourced graph** — doibio. (Graph / TRL 3+.)
- ✅ **Co-authored published hydrophone event-detection at 95% accuracy on ONC data** — Sattar 2011. (Acoustic credibility / TRL 3.)
- ✅ **Multi-stage semi-supervised acoustic ML at archive scale** — ORCA-SLANG 2021. (Semi-supervised methodology / TRL 3.)
- ✅ **Named inventor on granted patent for the entity architecture** — US 10,936,582. (Novelty / TRL 3.)
- ⏳ **Cross-modal vessel re-ID combining gap + SAR + EO + acoustic** — TRL 2 at start; the 1a's research deliverable.

This package is unambiguous TRL 2 at minimum. The TRL 3 framing is defensible because the **critical function** (cross-modal observation produces a calibrated re-ID candidate with full provenance) is implementable using methodology the applicant has already published or implemented in the adjacent domains; the 1a is integration + adaptation, not invention from scratch.

---

## What should NOT appear in the bid (corrected from v1)

- **The BioBERT attention-heads binding-site project.** v1 portfolio audit could not locate this project in any code or notes. Not cited.
- **Any third-party foundation-model code as "Zax Analytics implementation."** Cited only as power-user experience (above).
- **"Paid by ONC" or "Orchive for Dewey"** — not citable from local artifacts (above).
- **OOR (Open Ocean Robotics) data, code, or letter of support** — applicant's spouse works at OOR; OOR is explicitly not on the bid; v1 already established this exclusion.
