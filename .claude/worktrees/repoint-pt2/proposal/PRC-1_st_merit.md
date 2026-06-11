# PRC-1 — Scientific & Technological Merit

> **Field cap:** 3,000 characters.
> **Score:** 10 pts. (a) Sound S/T evidence + (b) State-of-the-art. Both = 10; one = 5.

---

## Draft (workspace markdown — strip headings before submission)

**(a) Sound S/T evidence.** Methodology grounded in peer-reviewed literature, granted IP, and reproducible measurements.

- *Lock-free nanosecond messaging* — LMAX Disruptor [B1a], Aeron [B1b]. Applicant's `planetar-broker` (predecessor `zbroker0`) extends this lineage with multi-transport fan-out, CRC32 WAL, zero-copy typed envelopes; predecessor reproduced p50 = 80–140 ns / p99 = 400–900 ns on commodity Linux, 1M-msg benchmark, 2026-04-27 (`docs/benchmark-2026-04-27.md`).
- *Event-sourced architecture* — Kreps, "The Log" (LinkedIn 2013) [B2a]; Apache Kafka [B2b]. Applicant's `doibio` implements this pattern for a 35-schema entity store, 18 months of iteration.
- *Cross-system entity resolution* — US Patent 10,936,582 [A8a]; 630-line confidence-weighted matching engine in `doibio/src/lib/identity-resolution.ts` (Levenshtein + exact-ID anchors, tuned thresholds merge ≥ 0.95 / link ≥ 0.80 / review ≥ 0.70).
- *Semi-supervised acoustic ML on continuous archives* — Sattar et al. PacRim 2011 [A1] (95 % accuracy on Ocean Networks Canada hydrophone data, Neptune Canada / CANARIE acknowledgement); Bergler et al. ORCA-SLANG, Interspeech 2021 [A2].
- *Dark-vessel detection from SAR + AIS* — DIU's xView3 [D1] (US DoD-component-funded competition); Park et al. *Science Advances* 2020 [D2]. Public precedent for tractability and AI advantage over classical baselines.
- *Calibrated uncertainty* — conformal prediction (Vovk-Gammerman-Shafer 2005 [E2]; Angelopoulos-Bates 2021 [E1]).
- *Auditory representation learning + unsupervised acoustic visualization* — Ness, Walters, Lyon (2012) [A3] (Walters/Lyon @ Google Research); applicant's SOM publications (PETRA / ICMC 2009) [A5a, A5b].

**(b) State-of-the-art.** planetar embodies and in places advances current SOTA:

- *Latency.* p50 = 80–140 ns SHM end-to-end on commodity Linux without kernel bypass or FPGA — well below the millisecond regime typical of ISR fusion buses.
- *Explainability.* Per-output `causation_id` lineage native to the system; analyst clicks from any output back to its raw inputs in one application. Structural rather than post-hoc XAI.
- *Composability.* Each viewer, detector, and the entity graph is a typed bus consumer; adding a modality is one ingress + one schema + (optionally) one viewer, no monolith change.
- *Acoustic methodology.* ORCA-SLANG / Sattar template is SOTA for archive-scale unlabeled hydrophone ML; the 1a extends it from cetacean-call to vessel acoustic signatures — structurally identical learning problem.
- *Identity resolution.* The Salesforce-inspired Party model is production-grade in commercial CRM; the variant covered by [A8a] and implemented in `doibio` is here first applied to maritime vessel-tracking ISR.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars. Trim Tier-2 evidence bullets if over.

## Cross-references to workspace

- All bracketed citations resolve in `06-REFERENCES.md`.
- Latency claim: `03-ARCHITECTURE.md` Layer 2.
- Identity-resolution scale: `03-ARCHITECTURE.md` Layer 4 + agent-1 audit.
- Patent: `04-PORTFOLIO.md` Tier 1B.
