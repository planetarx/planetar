# MC-1 — Current TRL and prior R&D

> **Field cap:** 3,000 characters.
> **Pass/fail.** Must accurately identify current TRL and describe R&D done to reach it.
> **Locked claim** (updated 2026-05-30, supersedes Q1): the solution — the learned cross-modal fusion model — is **TRL 2 at project start, advancing to TRL 3** at end-state.

---

## Draft (workspace markdown — strip headings before submission)

**Current TRL: 2.** The solution is a learned, self-supervised cross-modal fusion model that re-identifies a vessel after it disables AIS. The concept is formulated with strong component-level evidence — four working per-modality detectors, an identified AIS co-occurrence supervision signal, a built entity-resolution graph, and the applicant's peer-reviewed semi-supervised deep learning — but the integrated model is not yet built or demonstrated. The 1a builds and demonstrates it, advancing the solution to **TRL 3** (proof-of-concept demonstrated, including a live system at planetar.ca).

Prior R&D establishing the critical function:

(1) **Multi-modal ingest and per-modality detection (built).** Four broker-integrated services produce the typed observations the fusion model encodes: `planetar-ais` (live AIS), `planetar-sat` (Sentinel-1 SAR → CFAR + tracker, validated on a 433 Mpx scene), `planetar-eo` (EO → YOLO11n), `planetar-acoustic` (hydrophone → CAR-FAC + classifier). ~6 k LOC, open-source, broker-integrated.

(2) **Entity resolution with provenance (built).** `planetar-ontology` (zero-dependency TS, 30 tests) resolves cross-modal observations to canonical vessel identities with full lineage. It builds on the applicant's ~18-month `doibio` implementation and is a substantial development beyond the integrated-entity-view architecture of **US Patent 10,936,582** [A8a] — on which the applicant is a named inventor (Salesforce-assigned), not the patent holder.

(3) **The deep-learning approach the model builds on.** The applicant co-authored semi-supervised deep learning at archive scale on unlabelled hydrophone streams — ORCA-SLANG (Interspeech 2021) [A2] and Sattar et al. (IEEE PacRim 2011) [A1], the latter evaluated on Ocean Networks Canada data — the same scarce-label, continuous-stream regime the maritime fusion model operates in. The self-supervised joint-embedding design follows the I-JEPA family [I6].

(4) **Real-time, explainable substrate (built).** A provenance-tracked message bus, a zero-copy typed envelope, and an analyst shell give every inference an inspectable causal evidence chain and bit-exact replay — the deployable, accreditable surface the model runs on.

These — measured and peer-reviewed — demonstrate the critical function in adjacent settings (TRL 3). The 1a delivers the application-specific advance: the learned, self-supervised cross-modal dark-vessel re-identification model, TRL 2 → 3.

---

## Char-count budget

Target: ≤ 2,950 chars (50-char buffer). To be measured at red-team pass; trim items 4 and 5 if needed.

## Cross-references to workspace

- Bus measurement: `03-ARCHITECTURE.md` Layer 2 + benchmark appendix (W2 task).
- Envelope: `03-ARCHITECTURE.md` Layer 1.
- doibio scale + identity-resolution engine: `03-ARCHITECTURE.md` Layer 4 + agent 1 audit.
- Sattar 2011: `04-PORTFOLIO.md` Tier 1A + `06-REFERENCES.md` [A1].
- Patent: `06-REFERENCES.md` [A8a].
