# External Reader Briefing — IDEaS CH13 Component 1a Bid

You are being asked to do a **cold red-team pass** on a federal-government R&D proposal. The bid will be submitted on or before **2026-05-29** to Canada's Department of National Defence under the IDEaS Competitive Projects program.

This briefing is the only context you will have. Everything you need is in `~/github/planetarx/planetar/`.

---

## What this bid is, in 3 sentences

The applicant proposes a real-time multi-modal maritime situational-awareness platform — a Slack-style analyst UI + Palantir-style entity graph + an LMAX-Disruptor-class nanosecond message bus — for detecting and re-identifying vessels that have disabled their AIS transponder ("dark vessels"). The flagship 1a deliverable is an end-to-end demonstration on public maritime data (Sentinel-1 SAR, AIS, surface EO, passive hydrophone). The foundational architecture (bus + envelope + entity graph + analyst shell) already exists as working code; the 1a's research contribution is the cross-modal re-identification head that fuses these on the proven spine.

---

## What you are being asked to do

Read the eight narrative drafts in `proposal/` and the eight workspace docs at the top level (and the two supplements in `docs/`). Find every claim that:

1. **Won't survive a 6-year federal audit** (claims have a 6-year audit window after award; anything unverifiable kills the contract retroactively).
2. **Sounds like marketing** rather than measurement.
3. **Won't be understood by a non-CH13-domain reviewer** (CAF / DND reviewers may be technical but rarely have your specific domain knowledge).
4. **Contradicts a workspace doc** (the proposal narrative must be grounded in the workspace docs, which are the audit evidence).
5. **Misses a CH13 scoring opportunity** that the supporting workspace docs would otherwise enable.

Suggest *line-level fixes*, not generalities. Direct quotes + edit suggestions.

---

## Files to read, in suggested order (~2 hours total)

### Set 1 — Get oriented (~20 min)
- `README.md` — directory map, dashboard, canonical-spine table
- `01-CHALLENGE.md` — the CH13 rubric and scoring weights
- `02-STRATEGY.md` — the bid's thesis, condensed

### Set 2 — Verify the architecture claims (~25 min)
- `03-ARCHITECTURE.md` — the five layers (envelope → bus → detectors → entity graph → shell)
- `docs/benchmark-2026-04-27.md` — the citable measurement supplement
- `docs/compute-estimate.md` — backing for the $18 K cloud line

### Set 3 — Verify the credentials (~15 min)
- `04-PORTFOLIO.md` — applicant's publications, patents, and audit-defensible vs. oral-history split
- `06-REFERENCES.md` — the full citation list

### Set 4 — Read the narratives cold (~50 min, all 8 narratives ≤ 3,000 chars each)
- `proposal/MC-1_trl.md` — pass/fail, TRL claim
- `proposal/MC-2_alignment.md` — pass/fail, Essential Outcome compliance
- `proposal/PRC-1_st_merit.md` — 10 pts
- `proposal/PRC-2_novelty.md` — 20 pts
- `proposal/PRC-3_impact.md` — 20 pts
- `proposal/PRC-4_feasibility.md` — 20 pts
- `proposal/PRC-5_gba_plus.md` — 5 pts
- `proposal/PRC-6_desired_outcomes.md` — 15 pts

### Set 5 — Sanity-check budget + risk (~10 min)
- `proposal/PRC-7_budget.md` — cost tables
- `07-TIMELINE.md` — 6-month milestone plan + risk register

---

## Specific things to focus on

### Highest-priority

1. **Numbers-vs-source discipline.** Every measurement (latency, LOC, throughput) in a narrative must match a workspace doc (`benchmark-2026-04-27.md`, `compute-estimate.md`, the doibio audit, etc.). If a narrative says `p99 = 400–900 ns` and the benchmark file disagrees, that's a kill-the-bid-on-audit fact. Already had one of these caught in internal red-team; verify it stays caught.
2. **Patent assignee framing.** US Patent 10,936,582 was filed during applicant's Salesforce employment. Default-assignment rules typically vest such patents in the employer. The bid uses "applicant is named inventor on" / "applicant-named US Patent" — never claims ownership. If you see a stronger claim ("applicant holds" / "Zax Analytics owns"), flag it. Conversely, if applicant has a separate assignment record proving Zax / inventor ownership, the bid could be strengthened — but only with that record in hand.
3. **GBA Plus (PRC-5).** Internal red-team caught this section claiming sales4 already had ARIA / i18n / CVD-safe palette — none of that is in the codebase. Section was rewritten in future tense as M5 deliverables. Verify the future-tense framing holds throughout PRC-5; aspirational claims kill the bid in audit even if they get the score.
4. **"To applicant's knowledge"** qualifier on PRC-2 novelty claims. The bid claims first-of-its-kind composition. If you find prior art (a published paper, a commercial product, a defence-contractor prototype) that already does what planetar proposes, this is critical. Specifically check: maritime-ISR architectures combining a sub-microsecond bus + entity graph + Slack-style multi-viewer shell.
5. **CH13 rubric coverage.** PRC-6 claims all five desired outcomes are covered. Verify each by tracing the claim to its supporting workspace doc and (if measurement-based) to the benchmark file.

### Watch for

- **"First", "leading", "production-grade", "structural", "by construction"** — marketing words that need explicit grounding.
- **Bare numbers** without units or sample size ("80 ns" — over what N? of what?).
- **Tense slippage** — present tense ("planetar provides X") for things that are M3 deliverables.
- **Abbreviations the reviewer may not know** — CARFAC, CVD, SCM_RIGHTS, conformal prediction. The narratives target a CAF / DND technical reviewer, not a CS researcher.
- **Cross-narrative contradictions** — e.g., MC-2 says "fuses four production modalities + one stub", PRC-6 says "all five outcomes covered." Both can be true simultaneously, but the consistency between them must hold.

### What NOT to spend time on

- Workspace meta-notes — `08-OPEN-QUESTIONS.md` and the "Char-count budget" / "Cross-references" sections at the bottom of each narrative draft are stripped before submission. Do not red-team these.
- The directory listing in `README.md` — descriptive, not a claim.
- Citations like `[A1]` — the resolution lives in `06-REFERENCES.md`. Sample one or two; don't audit all of them.

---

## Known weak points the bid is currently aware of

These are tracked in `08-OPEN-QUESTIONS.md` Q9–Q12; you do not need to re-flag them, but you may comment if you have insight:

- **Q9.** Patent assignee verification on US 10,936,582 (above).
- **Q10.** Citation count on Ness et al. CRANK 2004 — currently quoted as "150 cites" in `04-PORTFOLIO.md`; we removed the specific count from PRC-4 already.
- **Q11.** Proposer hourly rate × hours × overhead % not yet locked in PRC-7.
- **Q12.** This pass — the W4 external red-team — is what you are doing.

---

## Time estimate

- 2 hours for a thorough first pass.
- 1 hour for line-level edit suggestions on whatever you flag.
- Optional: 30 min on whichever narrative you find weakest, deeper.

Total ~3.5 hours. The bid clears 70 / 100 well; the question is whether it clears 85 / 100, which is the threshold for serious contract negotiation. Your pass is gating that.

---

## Format for your feedback

Per narrative or workspace doc:

```
### [doc / narrative name]

CRITICAL (would kill the bid at audit):
- "[exact quote]" — [why it fails] — [proposed replacement]

IMPORTANT (would lose ~5+ scoring points):
- "[exact quote]" — [why] — [proposed replacement]

MINOR (style / clarity):
- "[exact quote]" → "[shorter or clearer]" (saves N chars)
```

Prefer specific over general. The applicant is technical and will accept blunt feedback. Tact costs words; words cost time; time is the constraint.

---

## Submission deadline reminders

- Hard deadline: **2026-06-02 14:00 EDT**.
- Target submission: **2026-05-29** (3-day buffer).
- DIP web-form submission, character-counted; the narratives' workspace markdown is stripped before pasting.
- Once submitted, the bid is final. There is a "replacement submission" path but use it only if essential.
