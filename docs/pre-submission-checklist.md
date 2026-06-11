# Pre-submission Checklist — DIP submission day workflow

Target submission day: **2026-05-29** (Friday, 3-day buffer to 2026-06-02 14:00 EDT deadline).

This file is the day-of-submission protocol. Print it; check items off as you go.

**Calendar (today: 2026-05-21, mid-W4):**

| Day | Date | Workstream |
|---|---|---|
| **T-7** | Fri 2026-05-22 | Final freeze (this section) |
| **T-3** | Tue 2026-05-26 | Strip + convert markdown → plaintext |
| **T-2** | Wed 2026-05-27 | DIP dry run + screenshots |
| **T-1** | Thu 2026-05-28 | Final review — DO NOT submit |
| **T-0** | Fri 2026-05-29 | **Submit** (morning attempt, afternoon verify) |
| **Buffer** | 2026-05-30 → 2026-06-02 | Hands off; emergency portal slack |

---

## T-7 (Fri 2026-05-22) — final freeze

**Already resolved (carry-forward sanity-check only):**
- [x] Q9 — US 10,936,582 patent assignee verified (Salesforce; applicant 1 of 19 inventors). Patent-language audit applied 2026-05-13. *Sanity-check: re-grep narratives for "applicant's US Patent" / "applicant has a patent" / "patented by the applicant" — should match 0.*
- [x] Q10 — CRANK [A7] citation count = 148 verified (Google Scholar 2026-05-14); byline corrected to 4-author canonical.
- [x] Q13 — A1/A2/A3/A4/A5a/A5b/A6/A7/A8a citation byline sweep complete; A2 and A6 fixed. *A8b still outstanding — see below.*

**User-blocking — must close this week:**
- [ ] **DIP registration approved** by 2026-05-24 (Sun). Was submitted under v1. *Fallback submission path triggers if unconfirmed.*
- [ ] **Q11 — Hourly rate × hours × overhead % locked.** PRC-7 Tables A and C currently use `$175/$200/$225` placeholders + 22.7% overhead placeholder.
- [ ] **Q13 — A8b US 16/264,391 USPTO verification.** Publication number, inventor attribution, status — verify at uspto.gov before T-7.
- [ ] **Full `doibio` → `doibio3` cleanup.** Minimal ~600-LOC reference impl committed; full ~20 k LOC cleanup is user-blocked. *Can ship without if time-boxed; doibio3 minimal version + the doibio path in `04-PORTFOLIO.md` already cover the entity-graph credential.*

**This week's pass — verify before freeze:**
- [ ] W4 external cold-reader pass complete (reader hand-off: `docs/external-reader-briefing.md` + `docs/built-services-inventory.md`).
- [ ] All 8 narratives re-measured against the dashboard char-count protocol after the 2026-05-21 built-state pass; all ≤ 3,000 plaintext. PRC-6 still tightest; PRC-4 + PRC-2 + MC-2 + PRC-5 were edited 2026-05-21.
- [ ] `docs/built-services-inventory.md` LOC totals re-verified against actual `wc -l` (the inventory was built 2026-05-21; numbers won't drift, but git fetch + re-count is a 30-second sanity check before submission).

## T-3 (Tue 2026-05-26) — strip and convert

For each of `MC-1, MC-2, PRC-1, PRC-2, PRC-3, PRC-4, PRC-5, PRC-6`:

- [ ] Take the body between `## Draft` and `## Char-count budget` headings.
- [ ] Strip markdown emphasis: `**bold**` → `bold`, `*italic*` → `italic`, `` `code` `` → `code`.
- [ ] Strip workspace-only headings (e.g., `**(a)**`, `**(b)**`, `**(c)**` if the DIP form has separate sub-fields; keep them if the form is one big text box).
- [ ] Verify no markdown headings (`##`, `---`) remain in the submission text.
- [ ] Verify no leading workspace notes (e.g., `> **Field cap:** 3,000 characters.` is workspace meta — strip).
- [ ] Re-measure character count on the stripped text. Target ≤ 2,950.
- [ ] Save each as a plain `.txt` with the narrative name in `proposal/submission/` (create folder).

For PRC-7:

- [ ] Convert the markdown tables into the format the DIP financial form expects (likely fields: total, per-milestone breakdown, per-category breakdown, labour hours per milestone). Mapping:
  - Table A (Total Budget by Cost Category) → DIP "Cost Category" fields.
  - Table B (Milestone Cost Schedule) → DIP "Milestone Costs" fields.
  - Table C (Direct Labour by Milestone) → DIP "Labour Hours" fields.
  - Table D (Non-Labour Costs by Milestone) → DIP "Non-Labour by Milestone" fields.
- [ ] Strip Table E (sanity / self-audit) — it's workspace-only.
- [ ] Verify SC-1 compliance one more time: M1 + M2 + M3 ≤ 70 % of total.

## T-2 (Wed 2026-05-27) — dry run

- [ ] Log into DIP. Open the proposal form. Note any field labels that differ from the rubric in `01-CHALLENGE.md`.
- [ ] Paste each plain `.txt` into its DIP field. Save draft. Verify DIP's character count matches your local count (within 1–2 chars for newline encoding).
- [ ] Spot-check three citations resolve (cite `[A1]`, `[A8a]`, `[B1a]` and verify the full text in `06-REFERENCES.md`).
- [ ] Do a DIP "preview" / "draft" save and read the rendered draft — does it look right? Are sections in the right order? Are any tables broken?
- [ ] Take screenshots of every saved page (audit trail).

## T-1 (Thu 2026-05-28) — final review

- [ ] Read every narrative one final time, in the DIP form, in submission order.
- [ ] Verify all numbers match the submitted text: `$157,924 total`, `Milestone 1 = 50.7 % (≤70%)`, `925 hrs @ $140/hr fully-loaded = $129.5K labour`, `$7,924 MacBook (Materials, M1)`, `~13 k LOC at start`, `30 tests`, `433-megapixel scene`, `95% ONC accuracy`, `US 10,936,582`. (Post-re-center the nanosecond/TCP latency figures are no longer in the scored narratives — don't re-introduce them.)
- [ ] Verify no `2026-04-24` dates appear (those are historical workspace dates only).
- [ ] Verify no score directives ("→ 10/10" etc.) appear.
- [ ] Verify no workspace headings ("Draft", "Char-count budget", "Cross-references") appear.
- [ ] Verify no `[TODO]` / `XXX` / placeholders.
- [ ] Verify financial tables sum correctly across rows and columns.
- [ ] DO NOT submit yet. Sleep on it.

## T-0 (Fri 2026-05-29) — submission

**Morning, 09:00 PT / 12:00 ET — first attempt.**

- [ ] Check DIP system status page for known outages.
- [ ] Re-open the proposal in the DIP form. Verify nothing has been corrupted overnight.
- [ ] Read MC-1 one final time. If it's wrong, the bid fails on pass/fail and never reaches scoring. **MC-1 is the kill criterion.**
- [ ] Click submit.
- [ ] Screenshot the submission confirmation. Save the DIP submission reference number to `08-OPEN-QUESTIONS.md` as a closed item.
- [ ] If submission fails for any reason: do not panic; the deadline is 4 days out. Capture the error, contact PWGSC IDEaS contracting authority (`tpsgc.paidees-apideas.pwgsc@tpsgc-pwgsc.gc.ca`), retry.

**Afternoon — verify.**

- [ ] Receive submission acknowledgement email from DIP.
- [ ] If no acknowledgement within 4 hours, log back in and verify submission status. Contact contracting authority if unclear.
- [ ] Save acknowledgement email + submission reference + screenshots to a permanent project archive.

## Buffer: 2026-05-30 through 2026-06-02

- [ ] Do not touch the proposal.
- [ ] Available for clarifications only if DIP requests them through the portal.
- [ ] Do not start work on the contract until award is announced (typically 3–6 months post-close).

---

## Items to keep in the project archive (post-submission)

- DIP submission reference number.
- Final plain-text versions of each narrative (`proposal/submission/*.txt`).
- DIP confirmation emails and screenshots.
- The benchmark output (`docs/benchmark-2026-04-27.md`) including `consumer-ultra` raw stdout if retained.
- Git tags on every repo at submission time: `~/github/planetarx/planetar-broker`, `~/github/planetarx/planetar-ui`, `~/github/planetarx/planetar-ais`, `~/github/sness23/zmesg`, `~/data/dev/doibio` — and predecessors `~/github/sness23/zbroker0`, `~/github/sness23/sales4` (commit SHA captures "what the bid was claiming about implementation state on submission day").
- This checklist with all boxes marked.

---

## Audit-prep reminders

The 6-year audit window starts at award (not submission). Items to keep retrievable for 6 years post-award:

- Frozen git SHAs of all repos cited.
- USPTO record for US Patent 10,936,582 (download PDF; USPTO records have changed format historically).
- Citation snapshots for the published references — Google Scholar exports for [A1, A2, A3, A4, A5a, A5b, A7].
- Sentinel-1 sample tile fetch logs proving public-data sourcing.
- ONC / NEPTUNE Canada acknowledgement quote in [A1].

Audit-defensible state on submission day = audit-defensible state for the next 6 years.
