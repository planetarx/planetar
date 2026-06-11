# planetar — IDEaS CH13 v2 proposal workspace

**Project:** Zax Analytics application to IDEaS Competitive Projects **Component 1a** for Challenge 13 (CH13) of CFP6: *"Multi-modal AI for advanced situational decisions"*.

**Applicant:** Steven Randolph Ness (PhD, CS/ML), Zax Analytics, solo founder.
**Target:** Component 1a — up to $250,000 CAD, up to 6 months, TRL 1–3.
**Deadline:** **2026-06-02 14:00 EDT** (hard).
**Today:** 2026-05-28 (~5 days to deadline; **1 day to target submission 2026-05-29**). **Current week: W5** (per `07-TIMELINE.md`).

---

## What planetar is

A **learned cross-modal fusion model** for maritime domain awareness. Six heterogeneous streams — AIS, SAR, EO, acoustic, RF, and textual maritime reports — are encoded into a shared embedding where one vessel's observations resolve to a single calibrated, explainable identity, even when its AIS beacon is dark. A working real-time, provenance-tracked substrate (typed bus, entity graph, analyst shell — already built) makes the model deployable, explainable, and replayable. See `docs/recenter-learned-fusion.md`.

**Flagship application for the 1a:** detection and re-identification of **dark vessels** — vessels that have disabled AIS to evade monitoring. The platform ingests the modalities that reveal a vessel *after* the beacon goes dark, cross-references them through a provenance-tracked entity graph, and presents the reconstructed track to an analyst in the viewer shell.

---

## Why this wins CH13

| CH13 desired outcome | planetar answer |
|---|---|
| Spatiotemporal alignment across modalities | **Learned** alignment in the fusion model's shared embedding; evidential uncertainty propagated + conformal calibration |
| Entity resolution + dynamic knowledge graph | US Patent 10,936,582 (applicant-named inventor); graph is a bus consumer, provenance tracked per-edge |
| Policy-aware provenance with lineage | Every message carries `correlation_id` / `causation_id` / `source`; WAL is append-only with CRC32 |
| Real-time AI-powered fusion pipelines | **The learned cross-modal fusion model** (self-supervised on AIS-on co-occurrence) running on a built provenance-tracked substrate on commodity hardware |
| Explainable outputs for operator trust | Every detection is a message with a causation chain; viewer clicks through to raw inputs |
| SWaP / edge deployment | Bus is a ~1.2k-line C binary (`planetar-broker`) with no dependencies beyond libc + protobuf-c |

---

## Directory map

```
planetar/
├── README.md                       ← this file
├── 01-CHALLENGE.md                 ✅ CH13 rubric + planetar→outcomes mapping
├── 02-STRATEGY.md                  ✅ platform pitch + doibio POC framing
├── 03-ARCHITECTURE.md              ✅ bus + envelope + entity graph + viewers
├── 04-PORTFOLIO.md                 ✅ credentials (Tier 1–3) + ONC honesty section
├── 05-DATASETS.md                  ✅ AIS, SAR, EO, hydrophone, RF + scenarios
├── 06-REFERENCES.md                ✅ all citations grouped by role
├── 07-TIMELINE.md                  ✅ proposal calendar + execution M1–M6 + risks
├── 08-OPEN-QUESTIONS.md            ✅ Q1–Q8 resolved + new W2/W3 items
├── docs/
│   ├── architecture-diagrams.md    ✅ Mermaid: 5-layer overview, dark-vessel sequence, bus internals
│   ├── benchmark-2026-04-27.md     ✅ 1M-message reproducible bus benchmark
│   ├── compute-estimate.md         ✅ bottom-up backing for $18 K cloud line
│   ├── external-reader-briefing.md ✅ briefing for the W4 cold-reader pass
│   ├── glossary.md                 ✅ CH13 / maritime / architecture / ML term reference
│   ├── pre-submission-checklist.md ✅ T-7 to T-0 day-of-submission protocol
│   └── requirements.md             ✅ consolidated requirements index (R1–R8) + open-gap tracker
└── proposal/                       ✅ all 9 narratives drafted, in-budget
    ├── MC-1_trl.md                 ✅ pass/fail
    ├── MC-2_alignment.md           ✅ pass/fail
    ├── PRC-1_st_merit.md           ✅ 10 pts
    ├── PRC-2_novelty.md            ✅ 20 pts
    ├── PRC-3_impact.md             ✅ 20 pts
    ├── PRC-4_feasibility.md        ✅ 20 pts
    ├── PRC-5_gba_plus.md           ✅ 5 pts
    ├── PRC-6_desired_outcomes.md   ✅ 15 pts
    └── PRC-7_budget.md             ✅ tables (rate × hours pending user lock)
```

---

## Relationship to v1 (zdefence/)

**v1** (`~/github/sness23/zdefence/`) — MAIA-MD: single-model JEPA fusion on SAR + AIS + EO + hydrophone. Kept intact; consulted for the shared CH13 rubric, dataset list, and portfolio content that doesn't change.

**v2** (this directory, `planetar/`) — platform pitch: **the bus is the product**; dark-vessel detection is the flagship demo riding on it. Reuses v1 portfolio facts, changes the architecture narrative wholesale.

The two approaches are **not being submitted in parallel**. planetar is the intended submission; v1 is kept for reference and for any language/facts that still apply.

---

## Canonical spine (predecessor benchmark 2026-04-27; planetar-broker TCP baseline 2026-05-14)

| Component | Repo | Role | Status |
|---|---|---|---|
| **Bus** | `~/github/planetarx/planetar-broker` | TCP + UDP + SHM, WAL with CRC32, lock-free CAS | **Working** (~1.2 k LOC C + 190-LOC `shm-consumer` test client; ports 12001/12002/12003 + `/tmp/planetar-broker.sock`). Predecessor `zbroker0` measured SHM **p50=80–140 ns, p99=400–900 ns over 1M-message benchmark, 1.8–1.9 M msg/s** (`docs/benchmark-2026-04-27.md`). planetar-broker TCP path measured 2026-05-14: paced 200 k msgs (~15 k msg/s), **p50 = 34 µs, p99 = 424 µs** (see `docs/benchmark-2026-04-27.md` "TCP path" addendum). |
| **Envelope** | `~/github/sness23/zmesg` | Binary: UUIDv7, ns timestamps, topic, correlation/causation | Working (`zmesg.h`, 260 LOC) |
| **Shell** | `~/github/planetarx/planetar-ui` | React 19 Slack/Discord/Quip/Palantir 4-pane UI with WS bridge to the broker | Working; latency-instrumented; predecessor `~/github/sness23/sales4` |
| **AIS ingress** | `~/github/planetarx/planetar-ais` | Node microservice; live AIS for Victoria BBox; one chat channel per MMSI | Working |
| **SAR ingress + detector** | `~/github/planetarx/planetar-sat` | Python: Sentinel-1 GRD fetch → CFAR + land-mask → IoU tracker → `sar.chip` + `track.update` envelopes | Working; 1,583 LOC Py + 5 test files; broker-integrated (TCP 12001, BE-prefixed zmesg); last commit 2026-05-15 |
| **EO ingress + detector** | `~/github/planetarx/planetar-eo` | Python: public webcam feeds (Victoria POC: CHEK, BC Ferries, ONC) → YOLO11n vessel detection → `eo.frame` + `eo.detection` envelopes | Working; 1,786 LOC Py; broker-integrated; last commit 2026-05-15 |
| **Acoustic ingress + detector** | `~/github/planetarx/planetar-acoustic` | Python: hydrophone (ONC / OrcaSound / archive) → CAR-FAC + Lyons SAI → CV classifier → `acoustic.{site,detect,psd,classify}` envelopes | Working; 2,621 LOC Py + 5 test files; broker-integrated; last commit 2026-05-15 |
| **Entity graph (built)** | `~/github/planetarx/planetar-ontology` | TS/Node zero-dep (native `node:sqlite`) entity graph; ingests bus envelopes → identity resolution → SQLite; phases P1–P5 done incl. dark-vessel kinematic match | Working; 2,323 LOC TS; **30 tests pass**; broker-integrated (TCP 12002); last commit 2026-05-18 |
| **Canonical-data-model registry** | `~/github/planetarx/planetar-registry` | JS zero-dep codegen: JSON Schema + TS interfaces + SQLite DDL + zmesg field dictionaries; AIS/SAR adapter examples | Working; 710 LOC JS; `node demo.mjs` (8/8 round-trip) + `node demo-fusion.mjs` (5/5 fusion scenarios); SSOT for schemas the sat/eo/acoustic detectors emit against |
| **Entity graph (POC / pattern donor)** | `~/data/dev/doibio` + US Patent 10,936,582 | Predecessor: identity resolution, provenance, event sourcing | **~20 k LOC TS, 35 schemas, 630-line identity-resolution engine, 18 mo iteration**; patent assignee verified Salesforce (Q9); applicant 1 of 19 named inventors |

---

## Key identifiers (from v1)

- **Solicitation number:** W7714-248676/013
- **CFP reference:** CFP6 CH13
- **Tender URL:** https://canadabuys.canada.ca/en/tender-opportunities/tender-notice/cb-395-41051453
- **Submission portal:** https://defence-innovation-portal.my.site.com/
- **Contracting authority:** PWGSC IDEaS Team — tpsgc.paidees-apideas.pwgsc@tpsgc-pwgsc.gc.ca
- **DIP registration:** submitted; awaiting approval (per v1 status)

---

## Status dashboard

| Task | Status |
|---|---|
| CFP / rubric understood | ✅ |
| DIP registration | ⏳ **re-verify approval by 2026-05-24** (3 days from now) — fallback path triggers if not approved by end of W4 |
| Architecture chosen | ✅ platform + dark-vessel |
| Canonical spine identified | ✅ planetar-broker + zmesg + planetar-ui + planetar-ais + planetar-sat + planetar-eo + planetar-acoustic + planetar-ontology + planetar-registry + doibio (predecessors: zbroker0, sales4) |
| Bus latency reproduced (initial) | ✅ 2026-04-24 (10k–100k msg) |
| Bus latency formal SHM benchmark (`zbroker0` predecessor) | ✅ 2026-04-27 (1M msg, taskset variant, throughput report) — p50 80–140 ns / p99 400–900 ns |
| planetar-broker TCP path indicative measurement | ⚠️ 2026-05-14 (paced 200 k msgs, ~15 k msg/s, i9-9900K) — p50 34 µs / p99 424 µs; **indicative only**, raw artifacts not preserved; the reproducible benchmark with persisted artifacts is the M1 deliverable. See `docs/benchmark-2026-04-27.md` addendum. |
| planetar-sat (SAR) built | ✅ 1,583 LOC Py + 5 tests; broker-integrated; CFAR detector validated on Sentinel-1 GRD; last commit 2026-05-15 |
| planetar-eo (camera) built | ✅ 1,786 LOC Py; YOLO11n on Victoria POC webcams (CHEK, BC Ferries Swartz Bay, ONC); broker-integrated; last commit 2026-05-15 |
| planetar-acoustic (hydrophone) built | ✅ 2,621 LOC Py + 5 tests; CAR-FAC + Lyons SAI + CV classifier pipeline; broker-integrated; last commit 2026-05-15 |
| planetar-ontology (entity graph) built | ✅ 2,323 LOC TS, zero-dep (native `node:sqlite`); P1–P5 done incl. dark-vessel kinematic match; 30 tests pass; broker-integrated (TCP 12002); last commit 2026-05-18 |
| planetar-registry (canonical data model) built | ✅ 710 LOC JS, zero-dep codegen; demos 8/8 + 5/5 pass; SSOT for `sat/eo/acoustic` envelope schemas |
| Eight workspace docs (01–08) | ✅ |
| All nine narrative drafts (MC-1, MC-2, PRC-1…7) | ✅ in budget after Q9 patent-language audit (2026-05-13), PRC-2 "existing IP" tightening (2026-05-14), and **2026-05-21 built-state pass** (PRC-4 rewritten to surface 4 newly-promoted services + TCP perf; MC-2/PRC-2/PRC-5 ontology-shipped acknowledgement; PRC-7 `planetar-broker` naming fix). Last dashboard-protocol counts (2026-05-14): MC-1 = 2922 / MC-2 = 2919 / PRC-1 = 2676 / PRC-2 = 2896 / PRC-3 = 2920 / PRC-4 = 2904 / PRC-5 = 2930 / PRC-6 = 2997. **Re-measure all 8 against the dashboard protocol at T-3 (2026-05-26)** after the 2026-05-21 edits — local counter estimates: PRC-4 ≈ 2920, PRC-2 ≈ 2960, MC-2 ≈ 2956, PRC-5 ≈ 2952 — all ≤ 3,000 cap, PRC-6 still tightest. |
| Cold red-team pass (internal) | ✅ critical bugs flagged + fixed (p99, patent framing, GBA+, fuses-five) |
| W4 external cold-reader pass | ⏳ 2026-05-18 – 2026-05-24, **3 days remaining** — reader still TBC; `docs/external-reader-briefing.md` + new `docs/built-services-inventory.md` both ready for hand-off |
| LOC verification (`planetar-broker.c` / predecessor `broker-unified.c`, `identity-resolution.ts`, doibio total) | ✅ ~1.2 k + 190 (shm-consumer) / 1,673 (predecessor) / 630 / ~20 k; full multi-repo inventory at `docs/built-services-inventory.md` (total ~13 k LOC of working planetar-* code at proposal start) |
| Compute estimate ($18 K backing) | ✅ `docs/compute-estimate.md` |
| Budget locked | ✅ **$157,924** at $140/hr fully-loaded × 925 hrs + $7,924 MacBook (2026-05-30); Milestone 1 = 50.7% (≤70%) |
| Patent assignee verified (US 10,936,582) | ✅ Salesforce-assigned (recorded 2020-12-11); applicant is 1 of 19 named inventors; "applicant-named inventor" framing confirmed audit-safe — see `08-OPEN-QUESTIONS.md` Q9 resolution + patent-language audit |
| CRANK [A7] citation count verified | ✅ 148 cites (Google Scholar, 2026-05-14); also corrected wrong byline in A7 — canonical is 4-author Ness/de Graaff/Abrahams/Pannu, not the 7-author list inherited from v1 notes. See `08-OPEN-QUESTIONS.md` Q10 |
| Citation-byline sweep [A1–A8b] (Q13) | ✅ 2026-05-14 — A2 ORCA-SLANG byline corrected (12 → canonical 8 authors); A6 thesis/arXiv split (arXiv:1307.0589 is a separate workshop paper, NOT the thesis) into A6a thesis + A6b workshop; A4 ACM Multimedia 139 cites confirmed + DOI added; A1/A3/A5a/A5b verified. **A8b US 16/264,391 NEEDS-VERIFICATION** — user action at uspto.gov before T-7 |
| Repos open-sourced on GitHub | ✅ 2026-05-15 — 6 `planetar-*` repos + `zmesg` public, `LICENSE` files committed; `zmesg` Apache-2.0 (header carve-out so bus publishers aren't copyleft-bound), the rest AGPL-3.0; all gitleaks-scanned clean (history + tree). `doibio3` created (public, AGPL-3.0, scanned clean) as a minimal ~600-LOC reference implementation of the entity-graph layer; **full ~20k-LOC doibio cleanup into `doibio3` pending before submission** (user). `doibio2` kept private |
| Final proposal submission | ✅ **SUBMITTED 2026-05-30, DIP ref CP6-132296** (Status: Submitted; Initial TRL 2 → end TRL 3; $157,924 per filed summary). See `docs/submission-record.md`. |

## Where we are vs. timeline

**W0, W1, W2, W3 complete** — all 9 narratives drafted in budget; benchmark + compute estimate + internal red-team + Q9 patent-language audit + Q10 CRANK byline + Q13 citation-byline sweep all done; 6 `planetar-*` repos + `zmesg` open-sourced. **Currently mid-W4 (~8 days to target submission 2026-05-29; ~12 days to hard deadline 2026-06-02).** Critical path now narrows to four items: (1) user-blocked budget rate × hours lock (PRC-7 — Q11); (2) user-blocked USPTO verification of A8b US 16/264,391 (Q13); (3) **W4 external cold-reader pass (2026-05-18 – 2026-05-24)** — reader still TBC, 3 days left in the window; (4) **DIP-registration approval re-verification by 2026-05-24** — fallback submission path triggers if unconfirmed. Documentation pass on 2026-05-21 surfaced four built but under-cited services (`planetar-sat`, `-eo`, `-acoustic`, `-ontology`) and a TCP perf baseline (p50 34 µs); see `docs/built-services-inventory.md`.
