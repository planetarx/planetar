# Requirements — consolidated gathering

**Purpose.** One-page index of every constraint the planetar bid must satisfy. Source docs remain authoritative; this file is the cross-reference and the gap list.

**Today:** 2026-05-13. **Target submission:** 2026-05-29. **Hard deadline:** 2026-06-02 14:00 EDT.

Each requirement is tagged `[REQ-x.n]` so external readers and downstream checklists can cite by id.

---

## R1 — Submission compliance (pass/fail at intake)

| ID | Requirement | Source | State |
|---|---|---|---|
| REQ-1.1 | Submit via Defence Innovation Portal (DIP), no other channel | `01-CHALLENGE.md` §Submission mechanics | DIP registration submitted in v1; **re-verify approval before W5** |
| REQ-1.2 | Registration ≥ 48 h before close | same | inherited from v1; verify carry-forward |
| REQ-1.3 | Submission is final once submitted (use replacement path only if essential) | same | n/a until submitted |
| REQ-1.4 | No attachments — citations must resolve open-source | same | USPTO record + arXiv + journal DOIs satisfy this; no internal-only PDFs cited |
| REQ-1.5 | Protected B or lower, no classified content | same | bid is unclassified by construction |
| REQ-1.6 | Each narrative ≤ 3,000 characters plaintext (MC-1, MC-2, PRC-1…6) | `01-CHALLENGE.md` §Char-count caps | all 9 drafts in budget; tightest headroom PRC-2/PRC-4/PRC-6 |
| REQ-1.7 | PRC-7 tables only, no narrative | same | satisfied |
| REQ-1.8 | Submitted plaintext stripped of markdown emphasis, workspace headings, score directives, `[TODO]` markers | `docs/pre-submission-checklist.md` T-3 / T-1 | scripted strip-pass not yet done; T-3 task |

## R2 — Mandatory criteria (pass/fail at scoring)

| ID | Requirement | Source | State |
|---|---|---|---|
| REQ-2.1 (MC-1) | Accurately identify current TRL (1/2/3) and describe R&D activities done to reach it | `01-CHALLENGE.md` §MC-1 | **TRL 2 → 3** framing (updated 2026-05-30, supersedes Q1); `proposal/MC-1_trl.md` + `submission/08–11` |
| REQ-2.2 (MC-2) | Describe the solution, its S/T basis, and how it meets each Essential Outcome | `01-CHALLENGE.md` §MC-2 | drafted in `proposal/MC-2_alignment.md`; covers four modalities + one stub |
| REQ-2.3 (Essential Outcome) | Fuse ≥ 2 heterogeneous data types into classification/detection/correlation outputs | `01-CHALLENGE.md` §Essential outcome | four production modalities + RF stub = two-modality bar cleared with margin |
| REQ-2.4 (SC-1 financial) | Total ≤ $250 K CAD **and** Milestone 1 ≤ 70 % of total | `01-CHALLENGE.md` §SC-1 | **$157,924** (≈63% of cap) / Milestone 1 = 50.7 % (`PRC-7_budget.md`; real entry `submission/20`); 19.3 pp margin |
| REQ-2.5 (Component 1a envelope) | TRL 1–3, ≤ 6 months, no GFP, no OOR, no DND personnel | `01-CHALLENGE.md` §Component 1a parameters | scope is solo, public-data, TRL 2→3 advance |

## R3 — Point-rated scoring (need ≥ 70/100 to be responsive)

| ID | Criterion | Pts | What earns full points | Source |
|---|---|---|---|---|
| REQ-3.1 | PRC-1 S/T merit | 10 | (a) sound S/T evidence + (b) state-of-the-art | `01-CHALLENGE.md` §PRC table |
| REQ-3.2 | PRC-2 novel & innovative | 20 | (a) new knowledge/tech + (b) enhanced vs SOTA + (c) future potential | same |
| REQ-3.3 | PRC-3 impact | 20 | (a) solves a gap + (b) enhances S/T capability + (c) matures the field | same |
| REQ-3.4 | PRC-4 feasibility | 20 | (a) achievable + (b) well-reasoned + (c) risks + mitigations | same |
| REQ-3.5 | PRC-5 GBA Plus | 5 | applied (not planned) to the technical solution | same |
| REQ-3.6 | PRC-6 desired outcomes | 15 | 100 % coverage of the five CH13 outcomes (spatiotemporal alignment, entity resolution, policy-aware provenance, scalable fusion + explainable outputs, SWaP/edge) | `01-CHALLENGE.md` §Desired outcomes |
| REQ-3.7 | PRC-7 cost alignment | 10 | realistic + milestone-aligned + proportional | `01-CHALLENGE.md` §PRC table |

**Target:** ≥ 85/100 (per `docs/external-reader-briefing.md` §Time estimate) — threshold for serious contract negotiation, not just responsiveness.

## R4 — Audit & provenance (6-year window after award)

| ID | Requirement | Source | State |
|---|---|---|---|
| REQ-4.1 | Every measurement traces to a workspace doc (latency, LOC, throughput) | `docs/external-reader-briefing.md` §Highest-priority #1 | benchmarks ↔ `benchmark-2026-04-27.md`; LOC re-verified 1,673 / 630 / ~20 k |
| REQ-4.2 | Every credential cite resolves on public record | same | Sattar 2011, ORCA-SLANG, Ness 2009, Orchive thesis, Google co-authorship, SOM papers — all on public record |
| REQ-4.3 | Patent framing never claims ownership beyond USPTO record supports | `08-OPEN-QUESTIONS.md` Q9 + `docs/external-reader-briefing.md` §HP #2 | bid uses "applicant-named inventor on US Patent 10,936,582" only |
| REQ-4.4 | ONC institutional history limited to what's locally verifiable | `08-OPEN-QUESTIONS.md` Q8 | "paid by ONC" / "Orchive for Dewey" absent from narrative; Sattar/Marsyas-in-Orchive lead |
| REQ-4.5 | Frozen git SHAs of all cited repos captured on submission day | `docs/pre-submission-checklist.md` §Audit-prep | planetar-broker, planetar-ui, planetar-ais, zmesg, doibio (and predecessors zbroker0, sales4) to be tagged at submission |
| REQ-4.6 | No aspirational claims in present tense (GBA+, ARIA, i18n claims are M5 deliverables) | `docs/external-reader-briefing.md` §HP #3 | PRC-5 rewritten future-tense in internal red-team |

## R5 — Execution deliverables (if awarded, M1–M6 over 6 months)

| ID | Milestone | Deliverable | Source |
|---|---|---|---|
| REQ-5.1 | M1 — Bus hardening | tagged `planetar-bus v0.1.0`, reproducible benchmark, architecture doc, CI on reference Linux box | `07-TIMELINE.md` §M1 |
| REQ-5.2 | M2 — Ingress + storage | four ingress adapters (AIS, Sentinel-1 SAR, EO, hydrophone), recorded datasets, replay-from-cursor verified | `07-TIMELINE.md` §M2 |
| REQ-5.3 | M3 — Detectors | five detector processes emitting typed bus messages (`ais.gap`, `sar.chip`, `eo.chip`, `acoustic.event`, `vessel.ReIDCandidate` scaffold) | `07-TIMELINE.md` §M3 |
| REQ-5.4 | M4 — Entity graph + cross-modal re-ID | re-ID candidate stream, queryable graph, conformal-calibrated fused score, calibration report | `07-TIMELINE.md` §M4 |
| REQ-5.5 | M5 — Viewer shell | `planetar-ui` running against the bus: map + timeline + entity-card + waveform + channel viewers | `07-TIMELINE.md` §M5 |
| REQ-5.6 | M6 — Demo + report + 1b | end-to-end Salish Sea synthetic scenario replayable from WAL; public-benchmark eval; 1b proposal packaged | `07-TIMELINE.md` §M6 |
| REQ-5.7 | TRL advancement | cross-modal fusion component TRL 2 → 3, on the already-TRL-3 spine | `08-OPEN-QUESTIONS.md` Q1 (locked) |

## R6 — Explicit non-requirements (scope guard)

`07-TIMELINE.md` §Scope discipline locks these as **out** of M1–M6: classified data, GFP, OOR data, DND personnel, frontier-scale training, production deployment, PSPC-IT integration, operator trials beyond the applicant, multi-analyst shell, FPGA / kernel-bypass work. Any reviewer feedback that would expand scope into one of these gets refused, not absorbed.

## R7 — Open requirements (gaps with owner + due date)

| Gap | Source | Owner | Due | Impact if unresolved |
|---|---|---|---|---|
| DIP registration approval re-verified | `README.md` dashboard | user | end of W4 (2026-05-24) | fallback submission path triggers |
| Hourly rate × hours × overhead % locked (Q11) | `08-OPEN-QUESTIONS.md` Q11 + `PRC-7_budget.md` §Lock-in | user | T-7 (2026-05-22) | PRC-7 still ships placeholder, scoring risk |
| ~~USPTO assignee verified for US 10,936,582 (Q9)~~ | ~~`08-OPEN-QUESTIONS.md` Q9~~ | ~~claude (USPTO web check)~~ | ✅ **RESOLVED 2026-05-13** — Salesforce-assigned (1 of 19 inventors). Patent-language tightening to-do tracked in Q9 resolution table | |
| ~~Apply patent-language tightening from Q9 audit table~~ | ~~`08-OPEN-QUESTIONS.md` Q9 patent-language audit~~ | ~~claude (after user OK)~~ | ✅ **APPLIED 2026-05-13** — all 6 edits in; MC-2/PRC-2/PRC-6 all ≤ 3,000 cap (PRC-6 tightest at ≈ 2 chars spare) | |
| ~~CRANK [A7] citation count verified or removed (Q10)~~ | ~~`08-OPEN-QUESTIONS.md` Q10~~ | ~~claude (Google Scholar)~~ | ✅ **RESOLVED 2026-05-14** — 148 cites (Google Scholar); also corrected wrong author list (was 7-author, canonical is 4-author Ness/de Graaff/Abrahams/Pannu) | |
| W4 external cold-reader pass (Q12) | `08-OPEN-QUESTIONS.md` Q12 + `docs/external-reader-briefing.md` | user (find reader) | 2026-05-18 – 2026-05-24 | gates 85/100 stretch; 70/100 already clears without it |
| Plain-text strip pass for DIP fields | `docs/pre-submission-checklist.md` T-3 | claude | 2026-05-26 | submission-day blocker if skipped |
| US Patent App 16/264,391 [A8b] verification | `08-OPEN-QUESTIONS.md` Q13 | user (uspto.gov) | T-7 (2026-05-22) | secondary IP credential; if can't confirm, drop A8b — bid does not hang on it |
| ~~Citation-byline sweep of [A1–A8b] (Q13)~~ | ~~`08-OPEN-QUESTIONS.md` Q13~~ | ~~claude~~ | ✅ **RESOLVED 2026-05-14** — A2 ORCA-SLANG byline corrected (12-author → canonical 8-author); A6 thesis/arXiv conflation split into A6a thesis + A6b workshop paper; A8b flagged NEEDS-VERIFICATION (separate row above) | |

## R8 — Status map (where each requirement lives)

- **Compliance** (R1) → `01-CHALLENGE.md` §Submission mechanics + `docs/pre-submission-checklist.md`
- **Mandatory** (R2) → `proposal/MC-1_trl.md`, `proposal/MC-2_alignment.md`, `proposal/PRC-7_budget.md` Table E
- **Scoring** (R3) → `proposal/PRC-1…6_*.md` narratives + char-count budgets at bottom of each draft
- **Audit** (R4) → `04-PORTFOLIO.md` audit-defensible vs oral-history split; `docs/benchmark-2026-04-27.md`; `06-REFERENCES.md`
- **Execution** (R5) → `07-TIMELINE.md` §B + `03-ARCHITECTURE.md` §5 layers
- **Out-of-scope** (R6) → `07-TIMELINE.md` §Scope discipline
- **Gaps** (R7) → `08-OPEN-QUESTIONS.md` Q9–Q12 + `README.md` dashboard

---

**How to use this file.** External reader: walk R2 → R3 → R4 and confirm each requirement maps to a narrative claim that survives audit. User: drive R7 to zero by T-7 (2026-05-22). Claude (future sessions): start here before touching narratives — every edit should preserve a requirement match, not regress one.
