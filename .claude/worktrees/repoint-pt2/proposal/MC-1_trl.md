# MC-1 — Current TRL and prior R&D

> **Field cap:** 3,000 characters.
> **Pass/fail.** Must accurately identify current TRL and describe R&D done to reach it.
> **Locked claim** (per `08-OPEN-QUESTIONS` Q1): TRL 3 at project start.

---

## Draft (workspace markdown — strip headings before submission)

**Current TRL: 3** (analytical and experimental critical-function proof-of-concept demonstrated).

The critical function planetar advances is **heterogeneous multi-modal data ingest, fusion through a typed message bus with end-to-end provenance, and explainable presentation in a multi-viewer analyst shell**. Prior R&D demonstrating this critical function:

(1) **Nanosecond message bus.** Applicant has implemented `zbroker0/broker-unified` — a unified C broker with TCP, UDP, and shared-memory transports, lock-free CAS reserves on the data path, durable write-ahead log with CRC32-protected 64 MB segments, recovery on restart, and cross-transport fan-out. Reproduced on commodity Linux 2026-04-27: SHM end-to-end **p50 = 80–140 ns, p99 = 400–900 ns** over 1,000,000-message benchmarks (`docs/benchmark-2026-04-27.md`). No kernel bypass, no FPGA. Implementation is in the LMAX Disruptor lineage [B1a].

(2) **Nanosecond-precision typed envelope.** Applicant has implemented `zmesg` — a zero-copy binary envelope (UUIDv7 id, three nanosecond timestamps, topic, source, schema/version, correlation_id, causation_id, opaque payload). Full per-message lineage is carried natively, not bolted on.

(3) **Patented entity-graph implementation.** Applicant has implemented `doibio` over ~18 months: ~20 k LOC, 35 JSON-Schema entity types, a **630-line identity-resolution engine** (Levenshtein + confidence-weighted matching across exact-ID, email/strong-id, fuzzy-name, and institutional anchors), event-sourced append-only log with SQLite + markdown projections, REST + WebSocket I/O. Applicant is named inventor on **US Patent 10,936,582** (granted 2021) [A8a], covering the integrated-entity-view architecture this implementation embodies.

(4) **Working multi-client analyst shell.** Applicant has implemented `sales4` — a Discord/Slack-style React 19 / TypeScript chat application with WebSocket server, microsecond-precision two-phase RTL latency instrumentation, CSV event logging, and a CLI load generator. End-to-end microsecond latency at the application layer is self-measured.

(5) **Maritime acoustic ML credentials.** Applicant is co-author on Sattar, Driessen, Tzanetakis, Ness, Page, "Automatic Event Detection for Long-Term Monitoring of Hydrophone Data" (IEEE PacRim 2011) [A1] — evaluated on **Ocean Networks Canada / NEPTUNE Canada operational hydrophone data at 95 % accuracy / 94.3 % F-measure**, with explicit Neptune Canada / CANARIE acknowledgement; and on Bergler et al., ORCA-SLANG (Interspeech 2021) [A2] — semi-supervised deep learning at archive scale on continuous hydrophone streams.

These items — measured, peer-reviewed, and patented — together demonstrate the critical function in adjacent settings, establishing TRL 3. The 1a R&D delivers the application-specific contribution: **cross-modal AIS-off vessel re-identification** composed on the proven spine, advancing that specific component from TRL 2 to TRL 3 over six months.

---

## Char-count budget

Target: ≤ 2,950 chars (50-char buffer). To be measured at red-team pass; trim items 4 and 5 if needed.

## Cross-references to workspace

- Bus measurement: `03-ARCHITECTURE.md` Layer 2 + benchmark appendix (W2 task).
- Envelope: `03-ARCHITECTURE.md` Layer 1.
- doibio scale + identity-resolution engine: `03-ARCHITECTURE.md` Layer 4 + agent 1 audit.
- Sattar 2011: `04-PORTFOLIO.md` Tier 1A + `06-REFERENCES.md` [A1].
- Patent: `06-REFERENCES.md` [A8a].
