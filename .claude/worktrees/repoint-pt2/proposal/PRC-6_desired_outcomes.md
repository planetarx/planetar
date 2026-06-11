# PRC-6 — Desired Outcomes coverage

> **Field cap:** 3,000 characters.
> **Score:** 15 pts. 100 % = 15, 50–99 % = 10, < 50 % = 5, none = 0. Target: **all five**.

---

## Draft (workspace markdown — strip headings before submission)

planetar covers all five CH13 desired outcomes by architectural design, not by add-on features. Each outcome maps to one or more existing implementation artefacts (bus, envelope, entity graph, viewer shell) plus citable methodology:

**(1) Spatiotemporal alignment, uncertainty propagation, confidence scoring across modalities.** Every observation enters the bus as a `zmesg` envelope carrying nanosecond `created_at_ns` from `clock_gettime(CLOCK_REALTIME)` plus `topic`, `correlation_id`, and `causation_id` — *temporal* alignment is wire-format guaranteed. *Spatial* alignment is performed in the entity-graph layer keyed on `(t, lat, lon)` plus per-modality measurement uncertainty. Cross-modal confidence uses log-score decomposition (prior + per-modality evidence + uncertainty + penalty) calibrated by **conformal prediction** [E1, E2] for distribution-free coverage guarantees. **Covered.**

**(2) Entity resolution + dynamic knowledge graph for persistent cross-domain tracking.** The entity-graph layer is grounded in **US Patent 10,936,582** *Integrated entity view across distributed systems* [A8a] (applicant-named inventor) and applicant's 18-month-iterated reference implementation `doibio` (~20 k LOC, 35 schemas, 630-line confidence-weighted identity-resolution engine). The 1a retypes the patented mechanism for vessels: canonical Vessel ↔ many VesselIdentifications (MMSI, IMO, RF fingerprint, acoustic signature, hull-OCR) ↔ ObservationSource provenance records. **Covered.**

**(3) Policy-aware fusion with AI-based provenance tracking, full lineage across classification levels.** Every bus message carries `correlation_id` and `causation_id`; every derived output's `causation_id` references the input envelopes that produced it. The WAL is append-only with CRC32-protected segments — bit-exact replay from any historical cursor. Classification-level metadata travels in `source` and an extensible flags field in the envelope; consumers filter on policy at subscription time. Lineage is structural to the system, not bolted on. **Covered.**

**(4) Scalable real-time AI fusion pipelines with explainable outputs for operator trust.** Bus p50 = 80–140 ns / p99 < 1 µs (`planetar-broker`; predecessor `zbroker0`, 2026-04-27); cross-transport fan-out (TCP/UDP/SHM); horizontal scale via additional consumers. Explainability is click-through navigation in `planetar-ui` from any output back through `causation_id` to its raw inputs — operator-facing, not external XAI. **Covered.**

**(5) SWaP and compute limits incorporated for edge deployment.** The bus is a ~1.2 k-line C binary (`planetar-broker`) with no dependency beyond libc + protobuf-c; the envelope is zero-allocation zero-copy; the shell is a browser-deliverable React app. The entire stack runs on laptop-class commodity Linux today — no FPGA, no kernel-bypass NIC, no SGX, no specialised silicon. SWaP is structural rather than retrofitted, addressing CH13's edge-deployment outcome (and the *Edge Fusion for Tactical Units* example) by construction. **Covered.**

All five desired outcomes covered.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars.

## Cross-references

- Outcome → implementation map: `01-CHALLENGE.md` table.
- Bus measurement: `03-ARCHITECTURE.md` Layer 2.
- Patent + doibio: `04-PORTFOLIO.md` Tier 1B + `03-ARCHITECTURE.md` Layer 4.
- Lineage: `03-ARCHITECTURE.md` Layer 1 + Layer 2 (WAL).
