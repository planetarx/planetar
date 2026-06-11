# 04 — Portfolio Audit: Citable Credentials

What's audit-defensible, what's working code, what's oral-history-only. Honesty bar: 6-year audit window post-award; everything claimed in the bid must survive that.

## Applicant

**Steven Randolph Ness, PhD**
- Company: **Zax Analytics** (incorporated in Canada).
- Canadian citizen.
- PhD: Computer Science, University of Victoria (2013). Thesis: *The Orchive*, supervised by George Tzanetakis (UVic CS, founder of Marsyas).
- Industry: Salesforce (ended 2021; named inventor on two US patents, both Salesforce-assigned). Currently Zax Analytics — AI for structural biology, scientific literature linking, and the planetar platform pitched here.
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

### B. Patent — entity-graph background IP (named inventor)

**US Patent 10,936,582 B2 (granted 2021-03-02; assignee Salesforce, Inc.).** *"Integrated entity view across distributed systems."* Applicant one of 19 named inventors — named-inventor credit, not assignee.

- Background for CH13 desired outcome #2 (entity resolution + dynamic knowledge graph) — the starting point the project's entity graph has since developed substantially beyond.
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

**Ness (2013).** *The Orchive: A System for Semi-Automatic Annotation and Analysis of a Large Collection of Bioacoustic Recordings.* PhD thesis, UVic. (Locatable via UVic DSpace; the separate companion workshop paper at arXiv:1307.0589 — "The Orchive: Data mining a massive bioacoustic archive", Ness/Symonds/Spong/Tzanetakis, ICML 2013 Workshop — is **not** the thesis itself.)

- The original semi-automatic-annotation system for the OrcaLab archive (decades of continuous hydrophone recordings off Hanson Island).
- Marsyas integration (the openmir codebase shells out to Marsyas binaries for feature extraction at production scale) — documented in code at `/home/sness/dev/orchive/openmir/`.
- Establishes long-term applicant engagement with **continuous-hydrophone ML at archive scale**, independent of any single paper.

**Collaborative-intelligence framing (load-bearing for CH13 "operator trust").** The Orchive is a *semi-automatic annotation* system — expert and crowd annotation drive the ML, and the human is designed *into* the loop rather than bolted onto the output. Concretely it was a **collaborative web platform** (Django / Celery / Marsyas; `~/dev/orchive/openmir`) over a **20,000-hour, 30-year** OrcaLab orca-call archive: expert researchers *and* citizen scientists (a casual-game annotation metaphor) added **18,000+ clip annotations**, training Marsyas/SVM classifiers (93–98.5% on segmentation / call-type tasks) whose outputs were shown back in the same interface — a closed human-in-the-loop loop. The thesis [A6a] frames this under **"Intelligence Augmentation" (§2.3)** and **"Citizen Science" (§2.4)**; the dedicated annotation-tooling paper is **[A9]**. Collaborative intelligence *shipped and peer-reviewed* — the strongest provenance available for planetar's operator-in-the-loop layer. This is the applicant's **shipped** precedent for **Collaborative Intelligence (CI)**: the CSCW-rooted design stance [G1–G7] that a human–AI system outperforms either alone when the operator is empowered by the AI from inside the loop. planetar realizes CI in its shell (Layer 5): the analyst's adjudications are first-class bus envelopes that (i) write back to the entity graph and (ii) close an active-learning loop [G5] refining detector / fusion thresholds. CI is what turns an "explainable output" into a *trust-calibrated joint decision* — the exact capability CH13 rewards ("explainable outputs for operator trust", "operational decision-making", "increase speed of decision making"). See `03-ARCHITECTURE.md` Layer 5.

### H. CRANK — automated scientific-computing pipelines

**Ness, de Graaff, Abrahams, Pannu (2004).** *"CRANK: new methods for automated macromolecular crystal structure solution."* Structure 12(10):1753–1761. **148 citations** (Google Scholar, verified 2026-05-14).

Demonstrates ability to ship complex automated pipelines at scale — execution credibility for PRC-4 (feasibility). Off-domain, so not cited as methodological precedent, but useful as evidence of "this person delivers production-grade automated systems."

### I. Salesforce UI patent

**US Patent 11,442,952 B2 (granted 2022-09-13; assignee Salesforce, Inc.; issued from App. 16/264,391).** *"User interface for commerce architecture."*

Secondary IP credential — applicant one of 11 named inventors, not assignee. Less directly relevant to CH13 than US 10,936,582, though its canonical-data-model matching / identity-reconciliation claims are entity-resolution-adjacent; shows an industry IP track record. (Verified on Google Patents 2026-06-01 — resolves the prior App.-16/264,391 verification flag.)

### J. MediaPipe — real-time on-device perception (SWaP / wearable fit)

The applicant has **worked with Google MediaPipe** [H1] for a few years and run experiments with it — the text-defined (`.pbtxt`) calculator-graph framework that runs vision / audio / sensor models **on-device** and lets a pipeline's networks be swapped by editing a text file, no recompile. Cited modestly as **practitioner familiarity**, in the same register as the foundation-model power-user paragraph — no product, LOC, or authorship claim, and not oversold.

> *[Framing per applicant 2026-05-30: keep MediaPipe **proposed**, not oversold. All planetar MediaPipe usage — the edge-perception runtime and the agentic graph-rewriting — is proposed 1a R&D, not built. No authored MediaPipe product was located under `~/github/sness23/` (`funchromium` = Chromium checkout; `sness23/mediapipe` = upstream fork with only automated Copybara import commits). The credential is simply: a few years of hands-on experimentation with the framework.]*

- **Why it lands in CH13.** MediaPipe is purpose-built for the *Edge Fusion for Tactical Units* example (audio + video + sensor on wearables under degraded connectivity) and Desired Outcome #5 (SWaP + compute limits for edge deployment). Honesty bar: cited as **working-practitioner experience**, in the same register as the foundation-model power-user paragraph — no authorship claim on the framework itself.
- **The novel R&D (PRC-2).** Because the perception graph is a text file, an **agentic controller can rewrite it at runtime** — a lighter network under power/thermal limits, a heavier classifier under high-threat conditions, a different modality mix when a sensor degrades — realizing the CFP's own goal of *"learned, adaptive fusion across modalities rather than static aggregation."* *[Confirmed 2026-05-30: proposed, not yet prototyped — a clean TRL-1–2 novelty deliverable for the 1a.]*

---

## Tier 3 — Background (mention if narrative space allows)

- *A physical map of the mouse genome* (Nature 2002, 435 citations) — rigorous scientific computing at scale.
- *Structure-based design of TEM-1 β-lactamase inhibitors* (Biochemistry 2000, 180 citations).
- Monte Carlo docking algorithms (1995–1997).
- *Phase refinement through density modification* (2007).

These establish depth; they're not on the methodological critical path for planetar.

---

## Working code that is the applicant's own

Verified by git remote URL (`github.com/sness23/...` and `github.com/planetarx/...`) and commit authorship — these are NOT third-party forks. The 6 `planetar-*` repos + `zmesg` were open-sourced 2026-05-15 (AGPL-3.0; `zmesg` Apache-2.0 with a header carve-out); all gitleaks-scanned clean. Full audit map: `docs/built-services-inventory.md`.

**Spine — bus + envelope + shell:**

| Project | Stack | LOC | Status / relevance to planetar |
|---|---|---|---|
| **planetar-broker** | C | ~1.2 k + 190 (shm-consumer) | Nanosecond bus on ports 12001/12002/12003 + `/tmp/planetar-broker.sock`. Predecessor `zbroker0` measured p50 = 80–140 ns / p99 = 400–900 ns over 1M-msg SHM, 2026-04-27 (`docs/benchmark-2026-04-27.md`). planetar-broker **TCP path baseline 2026-05-14: p50 = 34 µs / p99 = 424 µs at paced ~15 k msg/s**, i9-9900K, kernel 6.17 — raw artifacts preserved. Canonical spine. |
| **zmesg** | C (header-only) | ~260 | Binary envelope (UUIDv7, ns timestamps, topic, correlation/causation, source). 4-byte big-endian length prefix on TCP/UDP. Canonical wire format. |
| **planetar-ui** | TypeScript / React 19 / Vite | (in repo) | Slack/Discord/Quip/Palantir 4-pane analyst shell with WS bridge to the broker; µs-instrumented. Predecessor `sales4` comms-app. |

**Per-modality ingress + detectors (all broker-integrated at project start):**

| Project | Stack | LOC | Status |
|---|---|---|---|
| **planetar-ais** | Node / JS | (in repo) | Live AIS for Victoria BBox → per-MMSI bus channels. First ingress; pattern donor for the others. |
| **planetar-sat** | Python ≥3.11 (NumPy / SciPy / Rasterio / Shapely) | 1,583 + 647 (5 test files) | Sentinel-1 GRD fetch → CFAR + land-mask → IoU tracker → `sar.chip` / `track.update` envelopes. Validated on 433 Mpx scene. Last commit 2026-05-15. |
| **planetar-eo** | Python ≥3.11 (NumPy / OpenCV; optional torch + ultralytics) | 1,786 | Public webcam (CHEK / BC Ferries Swartz Bay / ONC / Hakai 2688×1512) → YOLO11n vessel detection → `eo.frame` / `eo.detection` envelopes. Last commit 2026-05-15. |
| **planetar-acoustic** | Python ≥3.11 (NumPy / SciPy / Soundfile; optional torch / onnxruntime) | 2,621 + 567 (5 test files) | Hydrophone source (synth / archive / ONC / OrcaSound) → CAR-FAC cochlear model + Lyons SAI → CV classifier → `acoustic.{site,detect,psd,classify}` envelopes. Last commit 2026-05-15. |

**Layer-4 entity graph (production successor to `doibio`):**

| Project | Stack | LOC | Status |
|---|---|---|---|
| **planetar-ontology** | TypeScript / Node ≥22.18, **zero npm dependencies** (native `node:sqlite`, native ESM) | 2,323 in 16 TS files | Ingests bus envelopes (TCP subscriber on 12002) → identity resolution → SQLite. **Phases P1–P5 complete** (zmesg codec + registry, identity resolution + merge, Object API, Action executor, kinematic match rules incl. **dark-vessel re-ID**). **30 tests passing**. Last commit 2026-05-18. Architecture doc: `~/data/vaults/docs/ARCH-planetar-ontology.md`. |
| **planetar-registry** | JS / Node ≥22, zero dependencies | 710 in 9 .mjs + 2 demos | Canonical-data-model codegen SSOT: registry JSON → zmesg field dictionaries + JSON Schemas + TS interfaces + SQLite DDL. The `sat`/`eo`/`acoustic` detectors emit envelopes conforming to schemas generated here. Demos: `node demo.mjs` (8/8 round-trip), `node demo-fusion.mjs` (5/5 fusion). |
| **doibio** (predecessor / pattern donor) | TypeScript / Node / SQLite | ~20 k across 35 schemas; `identity-resolution.ts` = 630 lines | The patent's working POC; ~18 months of iteration. Production successor `planetar-ontology` lifts the Party model + the 630-line resolution algorithm + the event-sourcing pattern, but is rebuilt zero-dep for the bus. doibio remains the **TRL evidence** (18-month-hardened reference implementation). Public-cleaned `doibio3` is the minimal v1; full ~20k LOC cleanup is user-blocked work pending before submission. |

**Methodology donors (referenced, not load-bearing on the canonical spine):**

| Project | Stack | Relevance to planetar |
|---|---|---|
| **crank3** | Python | Evidence aggregation for competing hypotheses; canonical JSON schema; log-score decomposition (prior + evidence + uncertainty + penalty); provenance. Direct analog for cross-modal vessel re-ID — fusion uses the same decomposition. |
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

For MC-1 the bid claims **TRL 3–4 for the bus + envelope + shell + entity graph + four per-modality detectors** and **TRL 2 for cross-modal dark-vessel fusion at project start, advancing to TRL 3 over the 1a**. Evidence on the table:

**Spine — shipped at project start:**
- ✅ **80–140 ns p50 SHM bus latency** — measured 2026-04-27, reproducible (predecessor `zbroker0`). (Bus / TRL 3+.)
- ✅ **34 µs p50 TCP path latency** — measured 2026-05-14, raw artifacts preserved (`planetar-broker`). (Bus deployment-realistic / TRL 3.)
- ✅ **Working React 19 multi-pane Slack/Discord/Quip/Palantir shell with WS bridge to the broker** — `planetar-ui` (predecessor `sales4`, µs-instrumented). (Shell / TRL 3.)
- ✅ **Working entity-graph service: identity resolution + merge + Object API + Action executor + dark-vessel kinematic re-ID rule** — `planetar-ontology` (P1–P5 shipped, 30 tests pass, zero-dep TS, broker-integrated). Successor to the doibio POC (~20 k LOC, 18 mo iteration, 630-line `identity-resolution.ts`). (Graph / TRL 3–4.)

**Per-modality detectors — shipped at project start:**
- ✅ **Live AIS ingress** — `planetar-ais`, Victoria BBox, per-MMSI bus channels. (TRL 3.)
- ✅ **SAR detector** — `planetar-sat`, Sentinel-1 GRD → CFAR + land-mask + IoU tracker → typed bus envelopes, validated on 433 Mpx scene. (TRL 3.)
- ✅ **EO detector** — `planetar-eo`, public webcam (CHEK / BC Ferries / ONC / Hakai) → YOLO11n vessel detection → typed bus envelopes. (TRL 3.)
- ✅ **Acoustic detector** — `planetar-acoustic`, hydrophone source → CAR-FAC + Lyons SAI → CV classifier → typed bus envelopes. (TRL 3.)

**Methodology / credential evidence:**
- ✅ **Co-authored published hydrophone event-detection at 95% accuracy on ONC data** — Sattar 2011. (Acoustic credibility / TRL 3.)
- ✅ **Multi-stage semi-supervised acoustic ML at archive scale** — ORCA-SLANG 2021. (Semi-supervised methodology / TRL 3.)
- ✅ **Named inventor on two granted Salesforce patents in the entity-resolution / identity-matching space** — US 10,936,582 & US 11,442,952. Background IP; the 1a's learned fusion model is the new, separately patentable work. (Novelty / TRL 3.)

**The genuine R&D — TRL 2 at start:**
- ⏳ **The learned cross-modal fusion model** — self-supervised on AIS-on co-occurrence, fusing AIS / SAR / EO / acoustic / RF / text with propagated uncertainty + conformal calibration — TRL 2 at start; the 1a's research deliverable (`vessel.v1.ReIDCandidate`, M3–M4).

The TRL claim is now even stronger than the v1 framing: the bid no longer asks the reviewer to take on faith that the single-modality detectors will materialize — four of them ship as broker-integrated working services on submission day. The 6-month 1a funds the **learned cross-modal fusion model** plus calibration plus the new text and RF encoders — that is the genuine new R&D, not the substrate.

---

## What should NOT appear in the bid (corrected from v1)

- **The BioBERT attention-heads binding-site project.** v1 portfolio audit could not locate this project in any code or notes. Not cited.
- **Any third-party foundation-model code as "Zax Analytics implementation."** Cited only as power-user experience (above).
- **"Paid by ONC" or "Orchive for Dewey"** — not citable from local artifacts (above).
- **OOR (Open Ocean Robotics) data, code, or letter of support** — applicant's spouse works at OOR; OOR is explicitly not on the bid; v1 already established this exclusion.
