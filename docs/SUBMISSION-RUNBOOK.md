# Submission runbook — IDEaS CFP6 CH13, Component 1a

**Audience:** Steven Ness, sole submitter. Read top-to-bottom; act in order.
**Companion:** `docs/pre-submission-checklist.md` (T-N day-of protocol — this runbook frames it).
**Last refreshed:** 2026-05-21.

---

## 0. Where we are right now

| Fact | Value |
|---|---|
| Today | **2026-05-21 (Thu)**, mid-W4 |
| Target submission | **2026-05-29 (Fri)** — 8 days |
| Hard deadline | **2026-06-02 14:00 EDT (Tue)** — 12 days, ~3-day buffer past target |
| Solicitation | W7714-248676/013 (CFP6 Challenge 13) |
| Portal | https://defence-innovation-portal.my.site.com/ |
| Contracting | tpsgc.paidees-apideas.pwgsc@tpsgc-pwgsc.gc.ca |
| Branch with this work | `docs/built-state-pass-2026-05-21` (pushed; PR-create URL: https://github.com/sness23/planetar/pull/new/docs/built-state-pass-2026-05-21) |

If anything in the table above is out of date when you read this, **stop and refresh it before continuing** — the rest of the runbook depends on these.

---

## 1. What only you can do (user-blocked, ordered by deadline)

These five items will not advance unless you sit down with them. Each has a hard or soft deadline; the runbook below assumes you've done them.

### 1.1 ⚠️ DIP registration approval — by **2026-05-24 (Sun)**

The CFP requires registration ≥48 h before close. Submission was made under v1; carry-forward needs re-verification.

**Action:**
1. Log into https://defence-innovation-portal.my.site.com/.
2. Verify your applicant profile lists Zax Analytics and shows "approved" / "active" status.
3. If status is **not approved**, email contracting (`tpsgc.paidees-apideas.pwgsc@tpsgc-pwgsc.gc.ca`) immediately citing solicitation W7714-248676/013 and registration date from v1.
4. Save a screenshot of the approved status — this is the audit anchor.

**Fallback (if unconfirmed by end of 2026-05-24):**
The proposal mentions a "fallback submission path" but it is not specified in v1 docs. Contact contracting same-day; do not wait. There is **no Plan C** that bypasses DIP — DIP is the only accepted channel (`01-CHALLENGE.md` §"Submission mechanics" #1).

### 1.2 Q11 — Lock hourly rate × hours × overhead %

Without this, PRC-7 Tables A and C remain placeholder. Current placeholders:

- Hourly rate: $175 / $200 / $225 (one of, choose)
- Overhead %: 22.7 % (placeholder)

**Decision points:**
- What's your defensible billable rate as a PhD CS / ML solo founder at Zax Analytics? Reference: SC-1 compliance requires M1+M2+M3 ≤ 70 % of total = $175 K.
- What overhead % is consistent with Zax's actual books? 22.7 % drives the $42 K G&A line; if your books show a different number, use that.

**Action:** edit `proposal/PRC-7_budget.md` Tables A and C to insert your locked numbers. Re-verify Table B (per-milestone) and Table E (sanity check) still balance.

### 1.3 Q13 — USPTO verification of A8b (US 16/264,391)

A8b is a secondary IP credential. WebFetch failed to confirm it from this session (USPTO and Google Patents are SPA-rendered).

**Action:**
1. Go to https://patentcenter.uspto.gov/.
2. Search application number `16/264,391`.
3. Verify: (a) publication number, (b) inventor list includes Steven Ness, (c) current status (granted? abandoned? pending?).
4. Update `06-REFERENCES.md` A8b with the verified info, or **remove A8b entirely** — the bid does not hang on it.

### 1.4 W4 external cold reader — name + hand-off

The W4 cold-reader pass window is 2026-05-18 → 2026-05-24. **3 days left.** Reader still TBC.

**Action:**
1. Pick a technically literate non-CH13 reader — a senior engineer outside the bid context. (One of your collaborators? An ex-colleague who knows distributed systems?)
2. Email them with the briefing pack:
   - `docs/external-reader-briefing.md` (what to look for)
   - `docs/built-services-inventory.md` (audit map for every claim)
   - The 9 narratives in `proposal/` (read in submission order: MC-1, MC-2, PRC-1..7)
3. Ask for a 1-hour read + a written list of "unclear claims" or "things that didn't land". Give them until **2026-05-23 (Sat)** to return feedback so you have Sun + Mon to rewrite.

### 1.5 doibio3 cleanup decision (~20 k LOC)

A minimal `doibio3` is already public. Full cleanup of the ~20 k LOC POC into a public repo is user-work.

**Decision:** is this worth the time, or do we ship without it?

- **Ship without:** the bid stands on `~/data/dev/doibio` (auditable from your laptop), the patent (USPTO public), and the minimal `doibio3` (GitHub public). PRC-1's 20 k-LOC / 35-schema / 630-line claim is defensible from any of those three.
- **Cleanup anyway:** stronger "open-source-replicable" framing for PRC-3, but it's a substantial time sink.

**Recommendation:** ship without unless you have a clear Sat/Sun window. Mark `requirements.md` R7 row resolved-via-decision either way.

---

## 2. The week ahead, day by day

Each day below summarises the goal; the detailed checklist lives in `docs/pre-submission-checklist.md`. **Print that file Friday morning.**

### Thu 2026-05-21 (today, T-8)

- Section 1 items above. All five.
- If cold reader is named today, you have margin.

### Fri 2026-05-22 (T-7) — final freeze

- Confirm everything in Section 1 above is done or has a decision.
- Re-measure all 8 narratives' plaintext char counts against the dashboard protocol (the local counter has ±100-char variance — re-verify with the strict protocol, not my counter).
- Q9/Q10/Q13 sanity-grep: search the proposal/ dir for "applicant's US Patent", "applicant has a patent", "patented by the applicant" → should match 0 lines.
- **No content edits after end of day.** The narratives are frozen.

### Sat 2026-05-23 (T-6) — cold reader returns

- Read the cold reader's feedback.
- Triage: which items are typos/clarity fixes (edit in place) vs structural concerns (decide whether to address in 1 day or live with).
- Apply triaged edits. Re-measure char counts after.

### Sun 2026-05-24 (T-5) — DIP registration confirmation deadline

- **Hard cutoff** for confirming DIP registration approval.
- If unconfirmed, you are in fallback territory — email contracting today, not tomorrow.
- If everything else is green, today is a rest day.

### Mon 2026-05-25 (T-4) — open day

- Buffer for any cold-reader edits that took longer than Saturday allowed.
- If clean, use today to print + read the entire proposal on paper, top-to-bottom, in submission order, **as a reviewer would**.

### Tue 2026-05-26 (T-3) — strip + convert

- This is the day you build `proposal/submission/*.txt` from the markdown drafts. Detailed protocol in `pre-submission-checklist.md` "T-3" section.
- For each narrative, apply the strip protocol, count chars, save as `.txt`.
- PRC-7: convert markdown tables into the format DIP expects (Cost Category, Milestone Costs, Labour Hours, Non-Labour by Milestone). Strip Table E (workspace-only).

### Wed 2026-05-27 (T-2) — DIP dry run

- Log into DIP. Open the proposal form. Paste each `.txt` into its corresponding field. **Save as draft, do NOT submit.**
- Verify DIP's character counter matches your local count (allow ±2 chars for newline encoding).
- Spot-check 3 citations resolve in `06-REFERENCES.md`.
- DIP "preview" / "draft" — read the rendered draft. Does it look right? Tables intact?
- **Take screenshots of every saved page.** Audit trail.

### Thu 2026-05-28 (T-1) — final review

- Read every narrative one final time, in the DIP form, in submission order.
- Sanity-check every load-bearing number against the submitted text: $157,924 total (≈63% of cap), Milestone 1 = 50.7 % (≤70%), 925 hrs @ $140/hr fully-loaded + $7,924 MacBook (Materials, M1), ~13 k LOC at start, 30 tests, 433-megapixel scene, 95% ONC accuracy, US 10,936,582. (Post-re-center the nanosecond/TCP latency numbers are no longer in the scored narratives.)
- No `2026-04-24` historical dates leaking into submitted text.
- No `[TODO]` / `XXX` / placeholders. No markdown `##` headings. No score directives.
- **Do not submit yet. Sleep on it.**

### Fri 2026-05-29 (T-0) — submission day

- **Morning 09:00 PT / 12:00 ET — first attempt.**
- Check DIP system status page for known outages.
- Re-open the proposal in DIP. Verify nothing has been corrupted overnight.
- Read MC-1 one final time. MC-1 is pass/fail — if MC-1 fails, the bid never reaches scoring.
- Click submit. Screenshot the confirmation. Save the DIP reference number.
- **Afternoon — verify acknowledgement email arrives** within 4 hours.

### Sat 2026-05-30 → Tue 2026-06-02 (slack buffer)

- Hands off. Available only for DIP-portal clarifications.
- The 3-day buffer exists for portal/system issues. If submission fails on Fri, you have Sat–Mon to retry. If clean, you have a weekend.

---

## 3. DIP portal mechanics

> ⚠️ I have **not** seen the actual DIP form layout in this session. The fields below are inferred from `01-CHALLENGE.md` §"Submission mechanics" and the IDEaS CFP rubric (which the form is structured against). Confirm field labels during the T-2 dry run on Wed 2026-05-27.

### 3.1 Login

URL: https://defence-innovation-portal.my.site.com/

The portal is Salesforce-Community-based (my.site.com is a Salesforce Community Cloud domain). Behaviour you can expect: session timeout ~30 min, paste fields with character counters, form auto-save likely (but verify), file attachments **not used** for CH13 narrative (no attachments allowed unless requested per `01-CHALLENGE.md` §"Submission mechanics" #6).

### 3.2 Form structure (expected)

| DIP field (likely label) | Our source file | Format | Cap |
|---|---|---|---|
| Mandatory Criterion 1 — TRL | `proposal/MC-1_trl.md` (Draft section, stripped) | plaintext | 3,000 chars |
| Mandatory Criterion 2 — Alignment | `proposal/MC-2_alignment.md` | plaintext | 3,000 chars |
| Point-rated Criterion 1 — S&T Merit | `proposal/PRC-1_st_merit.md` | plaintext | 3,000 chars |
| Point-rated Criterion 2 — Novelty | `proposal/PRC-2_novelty.md` | plaintext | 3,000 chars |
| Point-rated Criterion 3 — Impact | `proposal/PRC-3_impact.md` | plaintext | 3,000 chars |
| Point-rated Criterion 4 — Feasibility & Approach | `proposal/PRC-4_feasibility.md` | plaintext | 3,000 chars |
| Point-rated Criterion 5 — GBA+ | `proposal/PRC-5_gba_plus.md` | plaintext | 3,000 chars |
| Point-rated Criterion 6 — Desired Outcomes | `proposal/PRC-6_desired_outcomes.md` | plaintext | 3,000 chars |
| Cost — Total Budget by Category | `proposal/PRC-7_budget.md` Table A | financial form | (table) |
| Cost — Milestone Costs | PRC-7 Table B | financial form | (table) |
| Cost — Labour Hours per Milestone | PRC-7 Table C | financial form | (table) |
| Cost — Non-Labour by Milestone | PRC-7 Table D | financial form | (table) |

**On T-2 dry-run day, your first job is to match these inferred labels to the actual DIP labels and update this table.**

### 3.3 Strip protocol — markdown → plaintext

For each `MC-1, MC-2, PRC-1..6`:

1. Open the markdown file in `proposal/`.
2. Take the body between `## Draft` and `## Char-count budget` (everything between the two headers).
3. Strip:
   - `**bold**` → `bold`
   - `*italic*` → `italic`
   - `` `code` `` → `code`
   - `**(a)** / **(b)** / **(c)**` sub-section markers → drop entirely (DIP has subfields)
   - Workspace meta lines (anything starting with `> **Field cap:**` or similar)
4. Verify no `##` or `---` survives.
5. Count chars (your protocol; my local counter at `$CLAUDE_JOB_DIR/.../strip_count3.py` has ±100-char variance — don't rely on it for the cap check).
6. Save as `proposal/submission/<name>.txt`.

### 3.4 Field-by-field paste protocol (T-2 dry run)

For each field:

1. Open the `.txt` file. Select all. Copy.
2. Click into the DIP field. Paste.
3. **Verify DIP's character counter** equals your local count ±2 chars (newline encoding variance).
4. If DIP says **over cap**: do NOT trim by guessing — go back to the markdown, find the actual culprit (whitespace, smart quotes, em-dash conversion, hidden HTML), fix in the markdown source, re-strip, re-paste.
5. Click save / next.
6. Screenshot.

### 3.5 Submit (T-0, Fri 2026-05-29)

Submission is **final**. There's a "replacement submission" path but it requires the prior reference number and the original was likely rejected or withdrawn — don't plan to use it.

Order on submission day:
1. 09:00 PT / 12:00 ET — log in.
2. Verify draft is intact.
3. Read MC-1. If wrong, do not submit — fix first.
4. Click submit.
5. Screenshot the confirmation page (browser print-to-PDF is best — captures URL + timestamp).
6. Save the DIP reference number to `08-OPEN-QUESTIONS.md` as a closed item.

---

## 4. What to do if things go wrong

### 4.1 DIP portal outage

- Check https://canadabuys.canada.ca/en/tender-opportunities/tender-notice/cb-395-41051453 for outage notices.
- Email contracting: `tpsgc.paidees-apideas.pwgsc@tpsgc-pwgsc.gc.ca`. Cite solicitation `W7714-248676/013`. Ask for confirmation that the deadline will be extended for portal outage.
- Do **not** assume the extension. Keep retrying.

### 4.2 DIP rejects your text for being over the cap

- Don't trim blindly in the DIP field — that creates an off-protocol version that won't match your saved `.txt`.
- Go back to the markdown source, find the actual extra characters, fix, re-strip, re-paste.
- Common culprits: smart quotes (`"`/`"` are multi-byte), em-dashes auto-converted, non-breaking spaces, hidden `\r\n` line endings.

### 4.3 Submit button errors / page won't load

- Screenshot the error.
- Email contracting immediately. The 3-day buffer (Sat–Mon) exists for this.
- Do **not** wait to retry. Capture the failure state for audit, then try again ≤4 h later.

### 4.4 No acknowledgement email within 4 hours of submission

- Log back into DIP. Check the submission status.
- If status is "submitted" with a reference number, you're fine — email is just delayed.
- If status is "draft" or missing, you have a real problem — email contracting.

### 4.5 DIP registration not approved by Sun 2026-05-24

- Email contracting **same day**, cc'ing yourself.
- Cite the v1 registration date as evidence of timely registration.
- Ask whether registration approval can be expedited for solicitation `W7714-248676/013`.
- This is the only "fallback path" mentioned in proposal docs — there is no portal-bypass channel.

---

## 5. Post-submission

### 5.1 Buffer period — Sat 2026-05-30 → Tue 2026-06-02

- **Hands off the proposal.** Do not edit, do not log back into DIP to "look at it again."
- Available only for DIP-portal-initiated clarifications.
- Do **not** start work on the contract — award announcement is typically 3–6 months out.

### 5.2 What to save for audit (6-year window starts at award)

Save to a permanent project archive:

- **DIP submission reference number** + confirmation screenshots.
- **Final plaintext narratives** (`proposal/submission/*.txt`).
- **DIP acknowledgement email** + any subsequent portal correspondence.
- **Frozen git SHAs**, with tags, on every cited repo:
  - `~/github/planetarx/planetar-broker`
  - `~/github/planetarx/planetar-ui`
  - `~/github/planetarx/planetar-ais`
  - `~/github/planetarx/planetar-sat`
  - `~/github/planetarx/planetar-eo`
  - `~/github/planetarx/planetar-acoustic`
  - `~/github/planetarx/planetar-ontology`
  - `~/github/planetarx/planetar-registry`
  - `~/github/sness23/zmesg`
  - `~/data/dev/doibio`
  - Predecessors: `~/github/sness23/zbroker0`, `~/github/sness23/sales4`
  - Tag suggestion: `submission-2026-05-29`
- **USPTO record** for US 10,936,582 (download the PDF — USPTO record formats have changed historically).
- **Google Scholar citation snapshots** for [A1, A2, A3, A4, A5a, A5b, A7].
- **Sentinel-1 sample fetch logs** proving public-data sourcing.
- ~~TCP perf raw artifacts~~ — ⚠️ **Status as of 2026-05-28: artifacts were not preserved before the session ended.** The benchmark addendum in `docs/benchmark-2026-04-27.md` has been **re-framed as an indicative measurement** (not a formal reproducible benchmark); the M1 deliverable produces the CI-controlled reference-host TCP benchmark with persisted artifacts. No further action needed pre-submission.

### 5.3 If something in the bid turns out to be wrong post-submission

- You have a 6-year audit window. Inaccurate claims that survive 6 years are bad.
- If you discover a material error post-submission but pre-evaluation (rare but possible): contact contracting, explain, ask whether a correction can be filed.
- Post-evaluation: there is no fix. The bid is the bid.

---

## 6. Reference index — where to find things

| Need | File |
|---|---|
| What the proposal claims, top-to-bottom | `README.md` |
| The 9 narratives that get submitted | `proposal/MC-1_trl.md`, `MC-2_alignment.md`, `PRC-1_st_merit.md` … `PRC-7_budget.md` |
| What's actually built (audit map) | `docs/built-services-inventory.md` |
| Detailed T-N protocol | `docs/pre-submission-checklist.md` |
| Performance numbers we cite | `docs/benchmark-2026-04-27.md` (SHM + TCP) |
| Cold reader briefing | `docs/external-reader-briefing.md` |
| Open user-decisions | `08-OPEN-QUESTIONS.md` (Q11, Q13/A8b active) |
| All requirements tagged | `docs/requirements.md` |
| The CH13 rubric | `01-CHALLENGE.md` |
| Architecture deep-dive | `03-ARCHITECTURE.md` |
| Portfolio / credentials | `04-PORTFOLIO.md` |
| Datasets cited | `05-DATASETS.md` |
| Citations (A1–A8b, B1–B5, D1–D5, E1–E2) | `06-REFERENCES.md` |
| Timeline (proposal + execution M1–M6) | `07-TIMELINE.md` |
| Strategy + moat | `02-STRATEGY.md` |

---

## 7. One-page summary you can print on T-7

```
SUBMISSION RUNBOOK — DIP CH13 1a — STEVEN NESS

WHEN:    Fri 2026-05-29 (target)  |  Tue 2026-06-02 14:00 EDT (hard)
PORTAL:  https://defence-innovation-portal.my.site.com/
CONTACT: tpsgc.paidees-apideas.pwgsc@tpsgc-pwgsc.gc.ca
REF:     W7714-248676/013

T-7 Fri 22/05: registration confirmed, budget locked, A8b verified,
               cold reader briefed, narratives frozen
T-3 Tue 26/05: strip MD → .txt; convert PRC-7 tables; spot-check cites
T-2 Wed 27/05: DIP dry run, paste each .txt, save draft, screenshot
T-1 Thu 28/05: final read-through in DIP; sanity-check numbers; SLEEP
T-0 Fri 29/05: 09:00 PT submit; screenshot confirmation; verify email
T+1..T+4:      hands off; available for DIP clarifications only

ON FAILURE: email contracting same day; do not assume extension.
            buffer is Sat-Mon, not infinite.

NUMBERS:  SHM p50 80–140 ns / p99 400–900 ns (zbroker0, 2026-04-27)
          TCP p50 34 µs / p99 424 µs (planetar-broker, 2026-05-14)
          ~13 k LOC working planetar-* at start
          ~20 k LOC doibio POC + 630-line resolver
          ontology P1–P5, 30 tests pass
          $157,924 total (≈63% of cap); Milestone 1 = 50.7 %
          US 10,936,582 — APPLICANT-NAMED INVENTOR (not owner)
```
