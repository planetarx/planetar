# 08 — Open Questions

All eight questions resolved 2026-04-24. New questions will accumulate below as drafting surfaces them. ⚠️ = blocks next drafting step.

---

## ✅ Q1. TRL framing for MC-1 — RESOLVED 2026-04-24

Locked phrasing:

> "At project start, the bus (zbroker0), envelope (zmesg), and viewer shell (sales4/planetar-ui) are at TRL 3–4 — with a measured SHM end-to-end latency of 80–140 ns p50, durable WAL with CRC32, lock-free CAS reserves on the data path, and a working Slack-style multi-client shell. The **cross-modal dark-vessel fusion component** is at TRL 2 at project start. The 1a advances the fusion component from TRL 2 → TRL 3 by wiring it onto the already-proven bus and demonstrating it end-to-end on public data."

## ✅ Q2. Scope = option A (full demo) — RESOLVED 2026-04-24

Full-stack: bus + envelope + shell + five viewers + five detectors (`ais.gap`, `sar.chip`, `eo.chip`, `acoustic.event`, `vessel.ReIDCandidate`) + entity graph + end-to-end synthetic Salish-Sea scenario, all on public data. Every PRC narrative will be written to this scope.

## ✅ Q3. Naming — RESOLVED 2026-04-24

- System: **planetar**
- Bus: **planetar-bus** (repackaged `zbroker0`)
- UI: **planetar-ui** (repackaged `sales4/comms-app`)
- Envelope: **planetar.envelope.v1** protobuf on top of the existing zmesg binary wire format

The `~/github/sness23/zbroker0` and `~/github/sness23/sales4` directories remain on disk under their current names; "planetar-bus" / "planetar-ui" are the proposal-context names that will appear in narratives, diagrams, and any external-facing material.

## ✅ Q4. Patent framing — RESOLVED 2026-04-24

**Strong framing** confirmed: "The proposer's US Patent 10,936,582 *Integrated entity view across distributed systems* is the foundational novelty claim for the entity-graph layer." User noted: *"its basically what we are building on."*

User also flagged that **`~/data/dev/doibio` is a working POC of the entity architecture** the patent describes — planetar will rebuild it more carefully. The deep-dive agent that mined doibio for entity-architecture decisions completed; findings landed in `03-ARCHITECTURE.md` Layer 4 (Party model retyping, identity-resolution engine lift, event-sourcing pattern).

## ✅ Q5. Publications to cite — RESOLVED 2026-04-24 (additions to research)

Locked carry-overs from v1:
- ORCA-SLANG (Interspeech 2021)
- Sattar et al. 2011 (PacRim hydrophone)
- Ness et al. 2009 (ACM Multimedia, 139 cites)
- The Orchive (2013 thesis + arXiv 1307.0589)

**Added (background-agent verification complete):**
- Google paper — *Ness, Walters, Lyon (2012) "Auditory Sparse Coding"* in *Music Data Mining* (CRC Press) — confirmed and cited as [A3] in `06-REFERENCES.md`.
- SOM work — *Tzanetakis, Benning, Ness, Minifie, Livingston (2009) PETRA* and *Ness, Tzanetakis (2009) ICMC* — cited as [A5a] / [A5b] in `06-REFERENCES.md`, with `04-PORTFOLIO.md` §E expanding the relevance.

## ✅ Q6. Budget approach — RESOLVED 2026-04-24

Approach confirmed; specific labor figure to lock in W3 drafting:

| Category | CAD | Notes |
|---|---|---|
| Labor (proposer-scientist) | ~185k | Solo founder, six months; rate to confirm |
| Cloud / GPU compute | 18k | SAR tile fetches + SAR/EO detector fine-tuning |
| Datasets / licenses | 2k | Public datasets free; reserve for one commercial dataset |
| Software / tools | 3k | IDE, observability, minor paid services |
| Overhead / G&A | 42k | Zax Analytics overhead at conventional rate |
| Travel | 0 | CFP discourages |
| Subcontractors | 0 | Clean solo bid |
| **Total** | **250k** | |

## ✅ Q7. Benchmark claim — RESOLVED 2026-04-24

**Report measured-untuned numbers** (p50=80–140 ns, p99=350–480 ns, max 20 µs) as the bid claim. Tuned numbers may appear as an appendix if W2 has time. User: *"don't overclaim for sure, we want everything fully grounded by real numbers."* This becomes a project-wide discipline, not just a Q7 answer.

## ✅ Q8. Demo geography + ONC institutional history — RESOLVED 2026-04-24

**Salish Sea demo + arctic framing claim.** Plus a major credential the proposal will foreground: the user worked at Ocean Networks Canada (ONC) as a research assistant in the Tzanetakis lab integrating **Marsyas** into ONC's system (paid by ONC), and built a version of **The Orchive** for **Richard Dewey at ONC**. **Verification outcome (background agent, complete):** Sattar 2011 co-authorship on NEPTUNE Canada / ONC hydrophone data and the Marsyas-in-Orchive integration are locally verifiable and lead the bid; the "paid by ONC" and "Orchive for Dewey" claims could not be substantiated from local artifacts and are intentionally absent from the narrative (see `04-PORTFOLIO.md` "ONC institutional history — what the bid can and can't say").

This is potentially the strongest single credential in the bid — "paid by ONC to integrate ML into operational hydrophone infrastructure" is a credential almost no other CH13 bidder will have.

---

## Already-decided (captured here so it's traceable)

- ✅ Pivot from single-model JEPA (v1) to platform + application (v2 = planetar).
- ✅ Dark-vessel (AIS-off) detection is the flagship application.
- ✅ Solo applicant (Zax Analytics). No OOR, no co-applicants, no subcontractors.
- ✅ `zbroker0` (SHM ring + WAL, 80–140 ns p50 measured 2026-04-24) is the canonical bus.
- ✅ `zmesg` is the canonical envelope.
- ✅ `sales4` is the canonical shell (clean rewrite of the viewers, reuse the Slack/Discord-style chrome and WS server).
- ✅ Option A scope: full end-to-end demo across four modalities on public data.
- ✅ TRL framing: bus/shell TRL 3–4 at start; cross-modal fusion advances TRL 2 → 3 over the 6-month 1a.
- ✅ `v1` (zdefence/) preserved intact, referenced for unchanged content.
- ✅ Directory is `/home/sness/github/planetarx/planetar/`.

## To carry forward from v1 (not open questions, just TODO for next pass)

- ✅ Port `01-CHALLENGE.md` content — done.
- ✅ Port `04-PORTFOLIO.md` (applicant credentials) — done with verified ONC anchor (Sattar 2011) + Walters/Lyon Google co-authorship + SOM PhD-era papers.
- ✅ Port `05-DATASETS.md` (public data) — done with AIS-gap + hydrophone-anchored sections.
- ✅ Port `06-REFERENCES.md` — done; LMAX Disruptor [B1a], Aeron [B1b], Kreps "The Log" [B2a], Kafka [B2b] all present.

---

## New items surfaced in red-team pass (W2/W3 cleanup)

### ✅ Q9 — Patent assignee verification (US 10,936,582) — RESOLVED 2026-05-13

**Outcome: Salesforce-assigned (Q9 Outcome 1).** Verified via Google Patents (which mirrors USPTO assignment record):

- **Patent:** US 10,936,582 B2 — *Integrated entity view across distributed systems*
- **Inventors:** 19 named, including **Steven Ness**
- **Original assignee:** Salesforce.com, Inc. (assignment recorded **2020-12-11**, all 19 inventors → Salesforce)
- **Current assignee:** Salesforce, Inc. (same legal entity, post-2022 rename)
- **Grant date:** 2021-03-02

**Implication.** The bid's "**applicant-named inventor on** US Patent 10,936,582" framing stands as the audit-safe form. **No strengthening to "applicant holds" / "Zax Analytics owns" is supported by USPTO record.** Narrative usages that read as ownership (e.g., "applicant's US Patent", "applicant has a granted patent", "patented by the applicant") should be tightened to inventor-only attribution before W4 — see the patent-language audit below.

**Audit note.** 1 of 19 inventors is the precise framing if a reviewer presses. The credential is real (USPTO-verifiable) but it is *inventor credit*, not patent ownership.

### Patent-language audit (post-Q9, 2026-05-13) — ✅ APPLIED 2026-05-13

All 6 line-level edits applied 2026-05-13. Post-edit plaintext counts (markdown-stripped per `docs/pre-submission-checklist.md` T-3 protocol): **MC-2 = 2920 / PRC-2 = 2920 / PRC-6 = 2998** chars — all ≤ 3,000 cap. PRC-6 is now the tightest at ≈ 2 chars spare.

Original audit table for traceability:

| File:line | Current | Issue | Suggested |
|---|---|---|---|
| `proposal/MC-2_alignment.md:12` | "applicant's US Patent 10,936,582 [A8a]" | reads as ownership | "US Patent 10,936,582 [A8a] (applicant-named inventor)" or drop "applicant's" and let [A8a] carry attribution |
| `proposal/PRC-6_desired_outcomes.md:14` | "applicant's **US Patent 10,936,582**" | same | same |
| `proposal/PRC-2_novelty.md:14` | "US Patent 10,936,582 [A8a] and its ~18-month-iterated reference implementation `doibio`" | "its...reference implementation" implies possessive | reword: "US Patent 10,936,582 [A8a]; applicant's working POC `doibio` (~18 months, ~20 k LOC) is the reference implementation" |
| `02-STRATEGY.md:17` | "Already patented by the applicant" | reads as ownership | "Already covered by US 10,936,582 (2021), applicant-named inventor" |
| `03-ARCHITECTURE.md:140` | "The applicant has a granted patent on the architecture" | reads as ownership | "The applicant is named inventor on a granted patent covering the architecture" |
| `04-PORTFOLIO.md:162` | "✅ **Granted patent on the entity architecture** — US 10,936,582. (Novelty / TRL 3.)" | bullet drops inventor qualifier | "✅ **Named inventor on granted patent for the entity architecture** — US 10,936,582. (Novelty / TRL 3.)" |

PRC-3 (`applicant-named US Patent`, `applicant-named patent`), MC-1 (`Applicant is named inventor on`), PRC-1, and 03-ARCHITECTURE.md:208 / 02-STRATEGY.md:89 / 02-STRATEGY.md:108 are already in compliant form.

Char-count impact: tightening adds +12 to +20 chars per narrative. Headroom (per `README.md` dashboard): PRC-2 +10, PRC-6 +29 — PRC-2 will need compensating trim; PRC-6 fits. MC-2 headroom needs re-measuring after edit.

### ⚠️ Q10 — CRANK [A7] citation count

**Issue:** `04-PORTFOLIO.md` and `06-REFERENCES.md` cite 150 citations for Ness et al. (2004) CRANK. This figure originated in v1 portfolio notes and is asserted, not freshly verified. `06-REFERENCES.md` note 5 explicitly states cite counts will only be used where verifiable.

**Action needed before submission:** verify on Google Scholar (or equivalent). If verified, keep the number; if changed, update; if not verifiable, drop the specific number and use "well-cited" or remove entirely.

**Impact if dropped:** PRC-4 feasibility loses one supporting line ("applicant has shipped comparable production systems (CRANK [A7], 150 cites; …)"). Minor.

### Q11 — Final proposer hourly rate + overhead %

User-blocking. Two scalars determine PRC-7 Tables A and C:
- **Hourly rate.** Placeholder options staged in PRC-7: $175 / $200 / $225 per hour CAD.
- **Overhead %.** Placeholder 22.7 % (drives the $42 K G&A line).

**Action needed:** lock both. PRC-7 then becomes plug-and-play; SC-1 compliance is already verified at 48.8 % first-half spend.

### Q12 — External cold-reader red-team pass (W4)

Internal red-team done 2026-04-27 by an isolated agent; major issues (p99 number, patent framing, GBA+ aspirational claims, "fuses five" overshoot, score directives) all fixed. **External cold reader still scheduled for W4** (per `07-TIMELINE.md`) — recommend a technically literate non-CH13-domain reader (e.g., a senior engineer outside the bid context) for a fresh pass before submission.
