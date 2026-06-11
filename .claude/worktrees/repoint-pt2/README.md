# planetar — IDEaS CH13 v2 proposal workspace

**Project:** Zax Analytics application to IDEaS Competitive Projects **Component 1a** for Challenge 13 (CH13) of CFP6: *"Multi-modal AI for advanced situational decisions"*.

**Applicant:** Steven Randolph Ness (PhD, CS/ML), Zax Analytics, solo founder.
**Target:** Component 1a — up to $250,000 CAD, up to 6 months, TRL 1–3.
**Deadline:** **2026-06-02 14:00 EDT** (hard).
**Today:** 2026-05-13 (~20 days to deadline; ~16 days to target submission 2026-05-29). **Current week: W3** (per `07-TIMELINE.md`).

---

## What planetar is

A situational-awareness platform in the shape of **Slack × Palantir, rebuilt on a nanosecond message bus**. Every piece of information — SAR chips, AIS pings, hydrophone detections, RF emissions, EO frames, analyst chat, algorithm outputs — streams through the same bus as typed envelopes and surfaces in composable viewers (map, timeline, entity card, waveform, channel).

**Flagship application for the 1a:** detection and re-identification of **dark vessels** — vessels that have disabled AIS to evade monitoring. The platform ingests the modalities that reveal a vessel *after* the beacon goes dark, cross-references them through a provenance-tracked entity graph, and presents the reconstructed track to an analyst in the viewer shell.

---

## Why this wins CH13

| CH13 desired outcome | planetar answer |
|---|---|
| Spatiotemporal alignment across modalities | Nanosecond-timestamped envelopes on a single bus; viewers correlate by topic + time + entity id |
| Entity resolution + dynamic knowledge graph | US Patent 10,936,582 (applicant's); graph is a bus consumer, provenance tracked per-edge |
| Policy-aware provenance with lineage | Every message carries `correlation_id` / `causation_id` / `source`; WAL is append-only with CRC32 |
| Real-time AI-powered fusion pipelines | Demonstrated **p50 = 80–140 ns, p99 = 400–900 ns** over 1M-message benchmarks (zbroker0, 2026-04-27, `docs/benchmark-2026-04-27.md`) |
| Explainable outputs for operator trust | Every detection is a message with a causation chain; viewer clicks through to raw inputs |
| SWaP / edge deployment | Bus is a ~1.7k-line C binary with no dependencies beyond libc + protobuf-c |

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

## Canonical spine (formal benchmark 2026-04-27)

| Component | Repo | Role | Status |
|---|---|---|---|
| Bus | `~/github/sness23/zbroker0` | TCP + UDP + SHM, WAL, lock-free CAS | **Working. SHM p50=80–140 ns, p99=400–900 ns over 1M-message benchmark, 1.8–1.9 M msg/s throughput** (`docs/benchmark-2026-04-27.md`) |
| Envelope | `~/github/sness23/zmesg` | Binary: UUIDv7, ns timestamps, topic, correlation/causation | Working (`zmesg.h`, 260 LOC) |
| Shell | `~/github/sness23/sales4` | React 19 Discord/Slack-style UI + WS server + comms-cli | Working; latency-instrumented; roadmap item "swap in microsecond broker" matches |
| Entity graph (POC) | `~/data/dev/doibio` + US Patent 10,936,582 | Identity resolution, provenance, event sourcing | **~20 k LOC TS, 35 schemas, 630-line identity-resolution engine, 18 mo iteration**; patent applicant-named (assignee verification pending) |

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
| DIP registration | ⏳ submitted per v1 status; **re-verify approval before W5** — fallback path triggers if not approved by end of W4 (2026-05-24) |
| Architecture chosen | ✅ platform + dark-vessel |
| Canonical spine identified | ✅ zbroker0 + zmesg + sales4 + doibio |
| Bus latency reproduced (initial) | ✅ 2026-04-24 (10k–100k msg) |
| Bus latency formal benchmark | ✅ 2026-04-27 (1M msg, taskset variant, throughput report) |
| Eight workspace docs (01–08) | ✅ |
| All nine narrative drafts (MC-1, MC-2, PRC-1…7) | ✅ in budget post-Q9 patent-language edits (2026-05-13); plaintext counts MC-2 = 2920 / PRC-2 = 2920 / PRC-6 = 2998 (PRC-6 now tightest at ≈ 2 chars spare) |
| Cold red-team pass (internal) | ✅ critical bugs flagged + fixed (p99, patent framing, GBA+, fuses-five) |
| W4 external cold-reader pass | ⏳ scheduled 2026-05-18 – 2026-05-24 — reader TBC (`docs/external-reader-briefing.md` ready) |
| LOC verification (`broker-unified.c`, `identity-resolution.ts`, doibio total) | ✅ 1,673 / 630 / ~20 k |
| Compute estimate ($18 K backing) | ✅ `docs/compute-estimate.md` |
| Budget rate × hours locked (Q6) | ⏳ awaiting user (placeholder $175/$200/$225 staged) |
| Patent assignee verified (US 10,936,582) | ✅ Salesforce-assigned (recorded 2020-12-11); applicant is 1 of 19 named inventors; "applicant-named inventor" framing confirmed audit-safe — see `08-OPEN-QUESTIONS.md` Q9 resolution + patent-language audit |
| CRANK [A7] citation count verified | ⏳ Google Scholar check needed; current "150 cites" tagged with v1-notes provenance |
| Final proposal submission | ⏳ target 2026-05-29 (3-day buffer to 2026-06-02 deadline) |

## Where we are vs. timeline

**W0, W1, W2, W3 complete** — all 9 narratives drafted in budget; benchmark, compute estimate, and internal red-team done. **Currently mid-W3 (~16 days to target submission 2026-05-29).** Critical path now narrows to four items: (1) user-blocked budget rate × hours lock (PRC-7); (2) USPTO patent-assignee verification; (3) CRANK [A7] citation re-count; (4) **W4 external cold-reader pass (2026-05-18 – 2026-05-24)** — the only item with external-scheduling lead time. DIP-registration approval must also be re-verified before W5 (fallback submission path triggers if not approved by end of W4).
