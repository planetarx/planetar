# PRC-2 — Novel & Innovative

> **Field cap:** 3,000 characters.
> **Score:** 20 pts. (a) New knowledge/tech + (b) Enhanced vs SOTA + (c) Future potential. All three = 20; two = 15; one = 5.

---

## Draft (workspace markdown — strip headings before submission)

**(a) New knowledge / technology.** Three contributions are novel in CH13.

(1) *Architectural composition.* planetar is the first known maritime-ISR architecture composing: (i) a Disruptor-class nanosecond message bus with CRC32 WAL [B1a]; (ii) a patent-backed entity-graph with per-edge causation-id provenance [A8a]; (iii) a Slack-style multi-viewer shell where each viewer is a typed bus consumer. Each component exists individually; the *composition* — and the ns-level integration latency from forcing cross-component interaction through one bus — is new here.

(2) *Patent-backed entity-graph retyped for vessels.* US Patent 10,936,582 [A8a] (applicant-named inventor) and applicant's `doibio` POC (~20 k LOC, 35 schemas, 630-line identity-resolution engine) embody a Salesforce-inspired Party / PartyIdentification / PartySource model. The 1a retypes this for vessels — canonical Vessel entity, observed identifications (MMSI, IMO, RF fingerprint, acoustic signature, hull-OCR) — preserving the patented mechanism and adding cross-modal SAR / EO / acoustic / RF identification matchers as the new R&D within the existing IP.

(3) *Calibrated cross-modal vessel re-ID with explicit lineage.* `vessel.v1.ReIDCandidate` is itself a bus envelope whose `causation_id` references the source observations; its score uses log-score decomposition (prior + per-modality evidence + uncertainty + penalty, after applicant's `crank3` pattern), calibrated by conformal prediction [E1, E2] for distribution-free coverage guarantees. Joint calibrated cross-modal vessel re-ID at ns bus latency with native lineage is, to applicant's knowledge, not present in the literature.

**(b) Enhanced capability vs SOTA.**

- *Latency.* p50 = 80–140 ns / p99 = 400–900 ns over 1M-message benchmarks on commodity x86-64 — below the millisecond regime typical of ISR fusion buses. SWaP-relevant; no kernel bypass.
- *Explainability.* Native (causation chain on every output envelope) rather than post-hoc XAI. The reviewer / analyst clicks from any output back to its raw inputs in one application.
- *Composability.* Adding a modality is one ingress adapter + one schema + (optionally) one viewer — not a monolith change. CH13's other application examples (Arctic, airborne, edge tactical) plug in without architectural rework.
- *Auditability.* The WAL is the source of truth; "what did the system know at time T" is a cursor operation, not a backup-restore. Every output is bit-exact-reproducible from the input WAL.

**(c) Future potential.** The architecture generalizes beyond maritime without redesign: Arctic ISR (sat + RF + telemetry on the same bus + entity model), Airborne Multi-Sensor (radar + EO/IR + telemetry), Edge Tactical (audio + video + sensor — SWaP profile achievable on commodity x86 today; wearables in subsequent phases). Component 1b extends to a second domain; subsequent phases address operational deployment with CAF partners.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars. Tighten (a)(2) and (c) if over.

## Cross-references to workspace

- Composition novelty: `02-STRATEGY.md` "What's actually built today" + `03-ARCHITECTURE.md`.
- Patent: `04-PORTFOLIO.md` Tier 1B + `06-REFERENCES.md` [A8a].
- doibio lift: `03-ARCHITECTURE.md` Layer 4.
- Calibration: `06-REFERENCES.md` [E1][E2].
