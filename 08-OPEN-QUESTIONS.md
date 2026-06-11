# 08 — Open Questions

All eight questions resolved 2026-04-24. New questions will accumulate below as drafting surfaces them. ⚠️ = blocks next drafting step.

---

## ✅ Q1. TRL framing for MC-1 — RESOLVED 2026-04-24 (path updated 2026-05-14)

Locked phrasing:

> "At project start, the bus (planetar-broker, predecessor `zbroker0` measured SHM p50 80–140 ns), envelope (zmesg), and viewer shell (planetar-ui, predecessor `sales4`) are at TRL 3–4 — with durable WAL with CRC32, lock-free CAS reserves on the data path, and a working Slack-style multi-client shell. The **cross-modal dark-vessel fusion component** is at TRL 2 at project start. The 1a advances the fusion component from TRL 2 → TRL 3 by wiring it onto the already-proven bus and demonstrating it end-to-end on public data."

## ✅ Q2. Scope = option A (full demo) — RESOLVED 2026-04-24

Full-stack: bus + envelope + shell + five viewers + five detectors (`ais.gap`, `sar.chip`, `eo.chip`, `acoustic.event`, `vessel.ReIDCandidate`) + entity graph + end-to-end synthetic Salish-Sea scenario, all on public data. Every PRC narrative will be written to this scope.

## ✅ Q3. Naming — RESOLVED 2026-04-24

- System: **planetar**
- Bus: **planetar-broker** (`~/github/planetarx/planetar-broker/`, new C broker; `zbroker0` is the predecessor architecture)
- UI: **planetar-ui** (`~/github/planetarx/planetar-ui/`, new React shell; `sales4/comms-app` is the predecessor)
- Envelope: **planetar.envelope.v1** protobuf on top of the existing zmesg binary wire format

Both `planetar-broker` and `planetar-ui` are first-class repos in `~/github/planetarx/`, replacing the predecessor stack (`~/github/sness23/zbroker0`, `~/github/sness23/sales4`) in narratives, diagrams, and any external-facing material. Predecessors remain on disk for benchmark/lineage attribution.

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
- ✅ `planetar-broker` (SHM ring + WAL; predecessor `zbroker0` measured p50 80–140 ns on 2026-04-24, planetar-broker re-benchmark in progress 2026-05-14) is the canonical bus.
- ✅ `zmesg` is the canonical envelope.
- ✅ `planetar-ui` is the canonical shell (clean rewrite of the viewers, reusing the Slack/Discord/Quip/Palantir chrome and WS bridge; predecessor `sales4`).
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

**Follow-up edit (2026-05-14):** A 7th line was flagged after the initial 6-edit pass — `PRC-2_novelty.md:14` ended with "...adding cross-modal SAR / EO / acoustic / RF identification matchers as the new R&D within the existing IP." The phrase "the existing IP" read as applicant-owned IP. Cleanest fix was deletion (the preceding "preserving the patented mechanism" already establishes the patent context, making the trailing phrase both redundant and ambiguous). Now reads: "...adding cross-modal SAR / EO / acoustic / RF identification matchers as the new R&D." PRC-2 plaintext count dropped 2920 → 2896.

Char-count impact: tightening adds +12 to +20 chars per narrative. Headroom (per `README.md` dashboard): PRC-2 +10, PRC-6 +29 — PRC-2 will need compensating trim; PRC-6 fits. MC-2 headroom needs re-measuring after edit.

### ✅ Q10 — CRANK [A7] citation count — RESOLVED 2026-05-14

**Verified.** Google Scholar shows **148 citations** for Ness, de Graaff, Abrahams, Pannu (2004) *"CRANK: new methods for automated macromolecular crystal structure solution"* (Structure 12(10):1753–1761, DOI 10.1016/j.str.2004.07.018, PMID 15458625).

**Citation count updated** from "150 citations" → "148 citations (Google Scholar, verified 2026-05-14)" in both `06-REFERENCES.md` A7 and `04-PORTFOLIO.md` §H.

**Bigger fix surfaced during verification.** The author list in both files was materially wrong — listed as `Ness, McMullin, Pannu (A.J.S.), Storoni, Liu, Cowtan, Read` (7 authors), but the canonical paper has **4 authors**: `Ness, de Graaff, Abrahams, Pannu (N.S.)`. Storoni / Cowtan / Read are real crystallographers but on different papers (REFMAC5, Phaser). This was a serious audit risk — a reviewer cross-checking PubMed would have seen the byline mismatch immediately. Now corrected, with DOI + PMID added for traceability.

**Provenance.** Verified via PubMed (NCBI 15458625) for canonical citation + Google Scholar cluster `14525692363927032187` for cite count. Neither requires login.

### Q11 — Final proposer hourly rate + overhead %

User-blocking. Two scalars determine PRC-7 Tables A and C:
- **Hourly rate.** RESOLVED 2026-05-30: **$140/hr fully-loaded** (incl. overhead; total ask **$157,924** = ≈63% of the $250K cap, incl. $7,924 MacBook Materials).
- **Overhead.** Folded into the fully-loaded labour rate — no separate line (sidesteps §3.7 eligibility).

**Status:** RESOLVED. PRC-7 = **$157,924** ($140/hr fully-loaded + $7,924 MacBook; real entry is the 2-milestone form `submission/20`); Milestone 1 = 50.7 % (≤70%).

### ✅ Q13 — Citation-byline sweep of [A1–A8b] — RESOLVED 2026-05-14 (with one item flagged)

Triggered by the CRANK finding (Q10) — an inherited-from-v1 byline that turned out to be materially wrong. Did a defensive sweep of the remaining applicant publications to surface any sibling bugs before the W4 external reader sees the bid.

**Methodology.** For each [A*] entry, verify byline / title / venue / year against an authoritative public source (PubMed, ISCA archive, ACM DL, publisher page, arXiv, Google Scholar). Same standard as Q9 (USPTO record) and Q10 (CRANK).

**Findings.**

| Ref | Status | Notes |
|---|---|---|
| [A1] Sattar 2011 PacRim | ✅ Verified | Authors, title, venue match. IEEE Xplore doc 6032973. (Page range 668–674 not directly re-verified but consistent with multiple search hits.) |
| [A2] ORCA-SLANG (Interspeech 2021) | ⚠️→✅ **FIXED** | Bid byline was wrong: listed 12 authors (Bergler, Schröter, Cheng, Barucija, Schmitt, Bardeli, Hofer, Symonds, Spong, Ness, Schneider, Maier). ISCA archive confirms 8 authors: Bergler, Schmitt, Maier, Symonds, Spong, **Ness**, Tzanetakis, Nöth. Six names in the bid were not authors on this paper; two real authors (Tzanetakis, Nöth) were missing. Same class of bug as CRANK. Now corrected in `06-REFERENCES.md` A2 with DOI 10.21437/Interspeech.2021-616, pp. 2396–2400. **Narrative impact: zero** — narratives cite as "[A2]" or "Bergler et al., ORCA-SLANG", no hard-coded author lists. |
| [A3] Auditory Sparse Coding (2012) | ✅ Verified | Authors Ness/Walters/Lyon, book *Music Data Mining* eds Li/Ogihara/Tzanetakis, CRC Press 2012 — all match. (Walters/Lyon Google Research affiliation not directly re-fetched but well-established in public record.) |
| [A4] Ness 2009 ACM Multimedia | ✅ Verified + enriched | Authors, title, venue match. Google Scholar confirms **139 citations**. Added DOI 10.1145/1631272.1631393 + pages 705–708. |
| [A5a] PETRA 2009 | ✅ Verified | Authors Tzanetakis/Benning/Ness/Minifie/Livingston match (note: one search result spells "Livingstone" but ACM DL DOI 10.1145/1579114.1579117 is the canonical record). |
| [A5b] SOMba ICMC 2009 | ✅ Verified | Ness/Tzanetakis, title, venue match. |
| [A6] Orchive thesis + arXiv:1307.0589 | ⚠️→✅ **FIXED** | Bid conflated two different documents. arXiv:1307.0589 is **not** the PhD thesis — it is a 4-author ICML 2013 workshop paper titled "The Orchive: **Data mining a massive bioacoustic archive**" (Ness, Symonds, Spong, Tzanetakis). The PhD thesis is a separate UVic document with a different title ("A System for Semi-Automatic Annotation and Analysis of a Large Collection of Bioacoustic Recordings"). Split into [A6a] thesis (no arXiv ID; locatable via UVic DSpace) and [A6b] companion workshop paper (with arXiv:1307.0589). `04-PORTFOLIO.md` §G also corrected. |
| [A7] CRANK 2004 | ✅ Already fixed in Q10 | — |
| [A8a] US 10,936,582 | ✅ Already verified in Q9 | — |
| [A8b] → US 11,442,952 B2 | ✅ **RESOLVED 2026-06-01** | App. 16/264,391 granted as **US 11,442,952 B2** on 2022-09-13 (assignee Salesforce, Inc.; applicant 1 of 11 named inventors), verified via Google Patents. `06-REFERENCES.md` A8b and `04-PORTFOLIO.md` §I updated to the granted number; NEEDS-VERIFICATION flag cleared. See Q14. |

**Implication.** Three audit-critical byline / source bugs fixed (A2 wrong byline, A6 thesis/arXiv conflation, A7 wrong byline from Q10). The previously-unverified A8b credential is now resolved (granted US 11,442,952 B2 — see Q14). The remaining [A*] entries are clean.

**Audit posture.** All cited applicant publications now match their authoritative public records (or are explicitly flagged where not). A reviewer cross-checking any [A1–A6, A7, A8a] entry against PubMed / ISCA / ACM DL / arXiv / USPTO will see byline parity.

### Q12 — External cold-reader red-team pass (W4)

Internal red-team done 2026-04-27 by an isolated agent; major issues (p99 number, patent framing, GBA+ aspirational claims, "fuses five" overshoot, score directives) all fixed. **External cold reader still scheduled for W4** (per `07-TIMELINE.md`) — recommend a technically literate non-CH13-domain reader (e.g., a senior engineer outside the bid context) for a fresh pass before submission.

### ✅ Q14 — Patent framing v2: reduce footprint, reframe as background, add 2nd granted patent — RESOLVED 2026-06-01

Decision (applicant, for the **replacement submission**):
- **Both patents are named-inventor credit only, Salesforce-assigned** — never assignee. US 10,936,582 B2 (1 of 19 inventors, granted 2021-03-02) and US 11,442,952 B2 (1 of 11 inventors, granted 2022-09-13).
- **A8b resolved:** App. 16/264,391 granted as US 11,442,952 B2 (verified Google Patents); NEEDS-VERIFICATION flag cleared in `06-REFERENCES.md` A8b + `04-PORTFOLIO.md` §I.
- **Reduce footprint:** the patents were over-cited. Substantive (numbered) mention now kept in exactly **two scored narratives — MC-1 (prior R&D) and PRC-6 (#2 entity resolution)** — plus the references bibliography (`submission/26` / [A8a][A8b]). Removed from PRC-1, PRC-3, MC-2; "patented"/"patent-backed" stripped from MC-1 end-state. PRC-2 carries one generic, no-number reference framing the new model as patentable. *(The three milestone activity rows in `submission/20/21/23` still say "patent-backed entity graph" — left for the activities pass in a separate agent.)*
- **Reframe as starting point:** the patents are *background* the work has developed well beyond; the learned cross-modal fusion model is presented as **novel, separately patentable** foreground IP the applicant will protect (PRC-2). *(Caution: confirm this is consistent with the IDEaS/DND contract IP terms for foreground IP before relying on it.)*
- **Risk fix:** PRC-3's "Canadian-IP-grounded (US 10,936,582)" removed — a Salesforce-assigned (US) patent is not Canadian IP; sovereignty regrounded in the applicant's own open-source code + new, Canadian-owned IP (also `submission/27`).
- **Thinking docs reframed to match:** `02-STRATEGY.md` (verifiable-spine table + "what the patent claim points at" + the PRC-2 implication para + TRL-honesty line), `03-ARCHITECTURE.md` (Layer 4 heading/intro + "Why the … claim is now strong" + the ASCII diagram label), `01-CHALLENGE.md` outcome-2 mapping — patent demoted from novelty pillar to named-inventor background; the new fusion model is the novelty.
