# PRC-7 — Financial Proposal (Cost Tables)

> **Score:** 10 pts (assessed from the DIP 2-milestone Work Plan tables — `submission/20-work-plan-milestones.md`).
> **Screening (SC-1):** total ≤ $250,000 AND Milestone 1 ≤ 70 % of total.
> **Note:** the DIP form takes **two** milestones (Labour / Materials / Travel / Other Costs each). Our six internal milestones (M1–M6) map to Milestone 1 (M1–M3) and Milestone 2 (M4–M6).

---

## Locked values (2026-05-30)

- **Total ask:** **$157,924** of the $250,000 ceiling (≈63 % of cap).
- **Rate:** **$140 / hr, fully-loaded** (includes G&A/overhead — no separate overhead line; sidesteps §3.7 eligibility). 925 hours over 6 months (~154 hrs/month, full-time solo FTE).

---

## Table A — Total Budget by Cost Category

| Category | Amount (CAD) | % | Justification |
|---|---|---|---|
| Direct labour (925 hrs × $140 fully-loaded) | $129,500 | 82.0 % | Full-time PhD CS/ML founder; reasonable professional rate, well below the ~$200/hr top-of-market |
| Cloud / GPU compute | $18,000 | 11.4 % | Sentinel-1 fetches, model training/fine-tuning, eval runs, live planetar.ca hosting (`docs/compute-estimate.md`) |
| Software / tools | $1,500 | 0.9 % | IDE, observability, CI minutes |
| Datasets / licences | $1,000 | 0.6 % | Core datasets public/free; small reserve |
| Materials (MacBook Pro — local AI training + development) | $7,924 | 5.0 % | One-time dev/training laptop (Milestone 1) |
| Travel / Subcontractors | $0 | 0 % | Clean solo bid |
| **Total** | **$157,924** | **100 %** | ≈63 % of cap |

---

## Table B — Milestone schedule (internal M1–M6; DIP groups into 2)

| Internal | Description | Cost | % | DIP stage |
|---|---|---|---|---|
| M1 | Substrate + ingest hardening; text + RF ingest (incl. $7,924 dev/training laptop) | $22,024 | 13.9 % | **Milestone 1** |
| M2 | Per-modality encoders + shared embedding; AIS co-occurrence set | $24,750 | 15.7 % | Milestone 1 |
| M3 | Self-supervised fusion model — training (primary R&D) | $33,250 | 21.1 % | Milestone 1 |
| M4 | Uncertainty + conformal calibration; dark-vessel eval; entity retype | $35,350 | 22.4 % | **Milestone 2** |
| M5 | Explainable + human-in-the-loop analyst surface | $27,700 | 17.5 % | Milestone 2 |
| M6 | Integrated demo + live planetar.ca + evaluation + 1b package | $14,850 | 9.4 % | Milestone 2 |
| **Total** | | **$157,924** | 100 % | |

**SC-1:** DIP Milestone 1 (M1+M2+M3) = $80,024 = **50.7 %** ≤ 70 % → **passes** (19.3 pp margin). Milestone 2 = $77,900.

---

## Table C — Labour by milestone (925 hrs × $140 fully-loaded = $129,500)

| Internal | Hours | Labour (CAD) | DIP stage |
|---|---|---|---|
| M1 | 90 | $12,600 | M1 |
| M2 | 150 | $21,000 | M1 |
| M3 | 200 | $28,000 | M1 |
| M4 | 215 | $30,100 | M2 |
| M5 | 180 | $25,200 | M2 |
| M6 | 90 | $12,600 | M2 |
| **Total** | **925** | **$129,500** | |

DIP Milestone 1 labour = 440 hrs = $61,600; Milestone 2 labour = 485 hrs = $67,900.

---

## Table D — Other Costs (= $20,500: cloud $18,000 + software $1,500 + datasets $1,000)

DIP Milestone 1 Other Costs = $10,500 (cloud $9,000 + software $1,000 + datasets $500).
DIP Milestone 2 Other Costs = $10,000 (cloud $9,000 + software $500 + datasets $500).

Cross-check: labour $129,500 + materials $7,924 (MacBook Pro, M1) + other $20,500 = **$157,924**. ✅

---

## Cross-references

- **Real DIP entry:** `submission/20-work-plan-milestones.md` (the 2-milestone tables).
- Compute backing: `docs/compute-estimate.md`.
- **Pricing rationale (2026-05-30):** ~$158K chosen as the realistic/proportional sweet spot — merit-based scoring rewards proportionality, not lowest price; the bid is sustainable (no underscoping/burnout doubt) and ~63% of cap (no grant-maxing scrutiny).
