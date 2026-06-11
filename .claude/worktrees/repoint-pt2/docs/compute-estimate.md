# Compute Estimate — backing for the $18 K cloud line in PRC-7

This file is a **citable supplement** to PRC-7 Table A row "Cloud / GPU compute = $18,000". It models the compute spend bottom-up so the budget is defensible under audit (the red-team agent flagged this as a gap: "asserted with no model").

All figures CAD, exclusive of taxes. Pricing benchmarked against AWS Canada Central / GCP Montreal / Azure Canada Central public list prices as of 2026-04-27.

---

## Bottom-up by milestone

| Item | Quantity | Unit cost | Subtotal |
|---|---|---|---|
| **M1 — Bus hardening** | | | |
| CI runner (GitHub Actions–compatible) | 6 mo × small instance | $25 / mo | $150 |
| Reproducible benchmark runs | 50 GB-hr × $0.05 | | $25 |
| **M2 — Ingress + storage** | | | |
| Sentinel-1 GRD scenes (Copernicus is free) | 200 scenes | $0 | $0 |
| Working storage (compute + WAL ringbuffer) | 500 GB × 6 mo | $0.025 / GB-mo | $75 |
| Tile-fetch egress | 300 GB | $0.05 / GB | $15 |
| Cold-storage WAL archives | 1 TB × 6 mo | $25 / TB-mo | $150 |
| **M3 — Detector training (GPU)** | | | |
| `sar.chip` fine-tune (xView3-derived) | 80 GPU-hr (A10/L4-class) | $1.50 / hr | $120 |
| `eo.chip` fine-tune (Singapore Maritime / MODS) | 60 GPU-hr | $1.50 / hr | $90 |
| `acoustic.event` semi-supervised train | 80 GPU-hr | $1.50 / hr | $120 |
| **M4 — Entity graph + cross-modal re-ID** | | | |
| Cross-modal fusion experiments | 100 GPU-hr | $1.50 / hr | $150 |
| Conformal calibration runs (CPU) | 40 CPU-hr | $0.10 / hr | $4 |
| **M5 — Viewer shell** | | | |
| Dev cloud (small instance for `planetar-ui`) | 6 mo | $40 / mo | $240 |
| **M6 — Demo + replay + benchmark report** | | | |
| Synthetic Salish-Sea scenario generation | 50 GPU-hr | $1.50 / hr | $75 |
| Replay benchmark runs | 50 GB-hr | $0.05 | $25 |
| Public-benchmark evaluation runs | 100 GPU-hr | $1.50 / hr | $150 |
| **Always-on dev infrastructure (M1–M6)** | | | |
| CI runner (existing M1 line — already counted) | — | — | — |
| Dev workstation cloud (benchmark host) | 6 mo | $300 / mo | $1,800 |
| Object storage for archives + WAL replay | 2 TB × 6 mo | $25 / TB-mo | $300 |
| Network egress / ingress overhead | flat | | $200 |
| Container registry / image storage | flat | | $50 |
| **Subtotal — modeled essentials** | | | **~$3,740** |

---

## Why $18 K, not $4 K

The $18 K line carries deliberate contingency for items that are real but hard to model precisely six months in advance:

| Contingency category | Reserve | Rationale |
|---|---|---|
| GPU price volatility / cloud-provider markup | $2,000 | A10 / L4 spot pricing fluctuates ±50 % week to week; reservation pricing higher |
| Larger-than-expected fine-tune iterations | $4,000 | Acoustic and SAR detectors may need 3–5× the modeled GPU-hours if first-pass calibration underperforms (per `07-TIMELINE.md` risk register) |
| Multi-AZ / failover testing | $1,500 | Bus durability claims (WAL recovery, cross-transport fan-out) need two-AZ test environments |
| Commercial dataset reservation | $2,000 | If a single non-public dataset becomes critical (e.g., commercial AIS feed sample for higher-density training), reserved budget exists to acquire one |
| Third-party benchmark / load-test services | $1,500 | Tools like k6 Cloud, Grafana Cloud, observability services beyond free tier |
| Cloud bill overruns / surprise items | $3,260 | Standard 6-month-budget contingency at ~20 % of total |
| **Total contingency** | **~$14,260** | |

**Total compute = ~$3,740 (essentials) + ~$14,260 (contingency) = $18,000.**

---

## What's explicitly *not* in this line

- **Direct labour** is in PRC-7 Table A row 1 ($185 K) — separate.
- **Software licences** (IDE, observability paid tier) are PRC-7 row 4 ($3 K) — separate.
- **Datasets** (public are free; commercial reserve is here under contingency, not in PRC-7 row 3 which is $2 K) — kept honest.
- **DND / classified compute resources** — not allowed at 1a; not used.
- **Frontier-model pretraining** — not in scope; all detectors are fine-tunes of public baselines.

---

## Sensitivity analysis

If essential modeled spend lands at the modeled $3,740 with no contingency drawdown, the bid returns ~$14 K to the contracting authority at end of contract — acceptable under standard cost-reimbursement terms.

If essential modeled spend doubles (e.g., GPU-hours 2× over plan), the bid still has ~$10 K contingency remaining, well within the $18 K cap.

If essential spend triples (worst plausible — multiple modalities require extensive retraining), contingency exhausts at month 5; the M6 buffer (per `07-TIMELINE.md`) absorbs the timeline impact through scope reduction (e.g., waveform viewer to v0.1).

---

## Cross-references

- PRC-7 Table A row 2 (Cloud / GPU compute = $18,000): `proposal/PRC-7_budget.md`.
- M1–M6 milestone definitions: `07-TIMELINE.md` execution calendar.
- Risk register feeding contingency rationale: `07-TIMELINE.md` end of file.
- Detector definitions (SAR / EO / acoustic / re-ID): `03-ARCHITECTURE.md` Layer 3.
