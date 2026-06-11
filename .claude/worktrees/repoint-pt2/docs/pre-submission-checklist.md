# Pre-submission Checklist — DIP submission day workflow

Target submission day: **2026-05-29** (Friday, 3-day buffer to 2026-06-02 14:00 EDT deadline).

This file is the day-of-submission protocol. Print it; check items off as you go.

---

## T-7 days (W5 Monday) — final freeze

- [ ] DIP registration approved (Q-blocker; was submitted under v1).
- [ ] Hourly rate × hours × overhead % locked (Q11 in `08-OPEN-QUESTIONS.md`); PRC-7 Tables A and C have real numbers, no `$<RATE>` placeholders.
- [ ] Patent assignee verified on USPTO record for 10,936,582 (Q9). Bid framing aligned with assignee state.
- [ ] CRANK [A7] citation count verified or removed (Q10).
- [ ] External red-team feedback (Q12) incorporated; one cycle of revisions complete.
- [ ] All eight narratives' final char-counts ≤ 3,000 plaintext (re-measure after final edits).

## T-3 days (Tuesday) — strip and convert

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

## T-2 days (Wednesday) — dry run

- [ ] Log into DIP. Open the proposal form. Note any field labels that differ from the rubric in `01-CHALLENGE.md`.
- [ ] Paste each plain `.txt` into its DIP field. Save draft. Verify DIP's character count matches your local count (within 1–2 chars for newline encoding).
- [ ] Spot-check three citations resolve (cite `[A1]`, `[A8a]`, `[B1a]` and verify the full text in `06-REFERENCES.md`).
- [ ] Do a DIP "preview" / "draft" save and read the rendered draft — does it look right? Are sections in the right order? Are any tables broken?
- [ ] Take screenshots of every saved page (audit trail).

## T-1 day (Thursday) — final review

- [ ] Read every narrative one final time, in the DIP form, in submission order.
- [ ] Verify all numbers match: `p50 = 80–140 ns`, `p99 = 400–900 ns`, `~20 k LOC`, `630-line engine`, `1M-message benchmark`, `2026-04-27`, `35 schemas`, `1,673-line C broker`, `$250,000 total`, `48.8 % first-half`.
- [ ] Verify no `2026-04-24` dates appear (those are historical workspace dates only).
- [ ] Verify no score directives ("→ 10/10" etc.) appear.
- [ ] Verify no workspace headings ("Draft", "Char-count budget", "Cross-references") appear.
- [ ] Verify no `[TODO]` / `XXX` / placeholders.
- [ ] Verify financial tables sum correctly across rows and columns.
- [ ] DO NOT submit yet. Sleep on it.

## T-0 (Friday 2026-05-29) — submission

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
- Git tags on every repo at submission time: `~/github/sness23/zbroker0`, `~/github/sness23/zmesg`, `~/data/dev/doibio` (commit SHA captures "what the bid was claiming about implementation state on submission day").
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
