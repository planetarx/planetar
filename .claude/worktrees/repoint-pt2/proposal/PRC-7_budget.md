# PRC-7 — Financial Proposal (Cost Tables)

> **Field type:** tables only — no narrative.
> **Score:** 10 pts. Realistic + milestone-aligned + proportional = 10; minor gaps = 5; misaligned = 0.
> **Screening (SC-1):** total ≤ $250,000 AND first-half-milestones (M1+M2+M3) ≤ 70 % of total. **Budget cannot be front-loaded.**

---

## Lock-in needed from user

Two scalars determine everything below:

1. **Hourly rate (CAD).** Placeholder used: `$<RATE>` per hour. Suggested working numbers:
   - $175 / hr → 1,057 hours total → ~176 hrs/month, full-time-equivalent.
   - $200 / hr → 925 hours total → ~154 hrs/month, standard FTE.
   - $225 / hr → 822 hours total → ~137 hrs/month, conservative.
2. **Overhead %.** Placeholder used: 22.7 % effective (drives the $42 K G&A line). If Zax Analytics has a registered overhead % through CRA / federal contracting it should replace this.

Once the hourly rate and overhead are locked, the labour-hour-per-milestone table below is plug-and-play.

---

## Table A — Total Budget by Cost Category

| Category | Amount (CAD) | % of total | Justification |
|---|---|---|---|
| Direct labour (proposer-scientist) | $185,000 | 74.0 % | Hourly × hours per Table C |
| Cloud / GPU compute | $18,000 | 7.2 % | Sentinel-1 tile fetches, SAR/EO detector fine-tuning, evaluation runs |
| Datasets / licences | $2,000 | 0.8 % | All public datasets free; reserve for one commercial dataset if needed |
| Software / tools | $3,000 | 1.2 % | IDE, observability, CI runner minutes, ancillary paid services |
| Overhead / G&A | $42,000 | 16.8 % | Zax Analytics overhead at standard rate |
| Travel | $0 | 0 % | CFP discourages travel for 1a |
| Subcontractors | $0 | 0 % | Clean solo bid |
| **Total** | **$250,000** | **100 %** | At the cap; SC-1 satisfied |

---

## Table B — Milestone Cost Schedule (SC-1 compliance)

| Milestone | Description | Cost (CAD) | % of total | Cumulative % |
|---|---|---|---|---|
| **M1** | Bus hardening (`planetar-bus` v0.1.0) — repackage, CI, reproducible benchmark | $30,000 | 12.0 % | 12.0 % |
| **M2** | Four ingress adapters + WAL cold storage + replay verification | $42,000 | 16.8 % | 28.8 % |
| **M3** | Five detectors (ais.gap, sar.chip, eo.chip, acoustic.event, vessel.ReIDCandidate scaffold) | $50,000 | 20.0 % | **48.8 %** |
| **M4** | Entity graph + cross-modal re-ID research (primary research deliverable) | $52,000 | 20.8 % | 69.6 % |
| **M5** | Viewer shell (`planetar-ui`) — map, timeline, entity-card, waveform, channel | $42,000 | 16.8 % | 86.4 % |
| **M6** | End-to-end Salish-Sea demo + evaluation + 1b proposal package | $34,000 | 13.6 % | 100.0 % |
| **Total** | | **$250,000** | 100.0 % | |

**SC-1 first-half check:** M1 + M2 + M3 = $122,000 = **48.8 %** of total. ≤ 70 % cap → **passes**, with 21.2 percentage points of margin. Budget is back-weighted toward the research-heavy milestones M3–M5.

---

## Table C — Direct Labour by Milestone

Labour-only allocation (excludes overhead, cloud, software, etc.). At rate `$<RATE>` per hour:

| Milestone | Hours | Direct labour (CAD) | % of labour |
|---|---|---|---|
| M1 — Bus hardening | 110 | $19,250 ¹ | 10.4 % |
| M2 — Ingress + storage | 175 | $30,625 | 16.6 % |
| M3 — Detectors | 230 | $40,250 | 21.8 % |
| M4 — Entity graph + research | 245 | $42,875 | 23.2 % |
| M5 — Viewer shell | 195 | $34,125 | 18.4 % |
| M6 — Demo + report | 102 | $17,875 | 9.7 % |
| **Total labour** | **1,057** | **$185,000** | **100 %** |

¹ Worked example uses $175 / hr. Replace with locked hourly rate; hour totals remain.

---

## Table D — Non-Labour Costs by Milestone

| Milestone | Cloud | Software | Datasets | Overhead | Total non-labour |
|---|---|---|---|---|---|
| M1 | $1,000 | $1,000 | $0 | $4,750 | $6,750 |
| M2 | $4,000 | $500 | $1,000 | $7,000 | $12,500 |
| M3 | $5,000 | $500 | $500 | $9,250 | $15,250 |
| M4 | $4,000 | $500 | $500 | $9,000 | $14,000 |
| M5 | $2,000 | $500 | $0 | $7,375 | $9,875 |
| M6 | $2,000 | $0 | $0 | $4,625 | $6,625 |
| **Total** | **$18,000** | **$3,000** | **$2,000** | **$42,000** | **$65,000** |

Cross-check: $185,000 (labour) + $65,000 (non-labour) = **$250,000**. ✅

---

## Table E — Sanity / Self-Audit (workspace only — strip before submission)

| Check | Required | Actual | Pass? |
|---|---|---|---|
| Total ≤ $250,000 | $250,000 cap | $250,000 | ✅ at cap |
| First-half cost ≤ 70 % | M1–M3 ≤ $175,000 | $122,000 (48.8 %) | ✅ 21 pts margin |
| Travel for 1a | discouraged | $0 | ✅ |
| Subcontractor free | preferred for clean solo bid | $0 | ✅ |
| GFP / classified | not allowed at 1a | none | ✅ |
| Realistic hours | sustainable for solo FTE | 1,057 hrs / 6 mo (≈176 hrs/mo) | ✅ |
| Each milestone has labour + measurable deliverable | required | yes (per `07-TIMELINE.md`) | ✅ |
| Cumulative deliverable risk | M6 demo + report | back-loaded research weight in M3–M5 | ✅ |

---

## Cross-references

- Milestone definitions and exit criteria: `07-TIMELINE.md` execution calendar.
- Open question Q6 (rate framing): `08-OPEN-QUESTIONS.md`.
- Total + category split first introduced: `08-OPEN-QUESTIONS.md` Q6 resolution table.
