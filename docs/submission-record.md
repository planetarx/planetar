# Submission record — IDEaS CFP6 Challenge 13, Component 1a

**STATUS: ✅ SUBMITTED — replacement filed 2026-06-01 (authoritative).**

The bid was first filed 2026-05-30 (CP6-132296) and then **replaced 2026-06-01 with a revised version, `CP6-132484`** — the replacement is the authoritative, evaluated submission. Both summary PDFs are archived in the repo root (`CP6-132296_ProposalSummary.pdf`, `CP6-132484_ProposalSummary.pdf`).

| Field | Value |
|---|---|
| Proposal title | Planetar: A Learned Cross-Modal AI Model for Maritime Dark-Vessel Detection |
| **DIP reference number** | **CP6-132484** (Replacement Submission) |
| Replaces | CP6-132296 (original New Submission, 2026-05-30) |
| Element | Competitive Projects |
| Component | Component 1a |
| Submission category | Replacement Submission |
| Initial TRL | **TRL 2** (end-state TRL 3) |
| Status | **Submitted (replacement)** |
| **Proposal submitted** | **2026-06-01, 9:03 p.m.** (replacement) · original 2026-05-30, 8:28 p.m. |
| Hard deadline | 2026-06-02, 14:00 EDT (replacement filed ~17 h ahead) |
| Applicant | Steven Ness, Zax Analytics (Victoria, BC, Canada) |
| Portal | https://defence-innovation-portal.my.site.com/ |
| Solicitation | W7714-248676/013 (CFP6 Challenge 13) |

## What changed in the replacement (CP6-132484 vs CP6-132296)

- **Second patent added + patents reframed.** US 11,442,952 B2 added alongside US 10,936,582 B2; both framed as **named-inventor (Salesforce-assigned) background** the work develops beyond, not owned IP. Substantive patent mention reduced to MC-1 + PRC-6 + the Reference Documents; the new fusion model is presented as novel, separately patentable foreground IP (PRC-2). PRC-3's misleading "Canadian-IP-grounded (US 10,936,582)" removed — sovereignty regrounded in the applicant's own open-source code + new Canadian-owned IP.
- **PRC-1–PRC-4 converted from bullet fragments to inline-labelled prose** (DIP strips line breaks, so bullets had rendered as run-on dash lists in CP6-132296).
- **"Planetar" capitalized** throughout prose (URLs `planetar.ca` and `planetar-*` repo names left lowercase).
- **Granular work plan:** Milestone 1 = 6 activities, Milestone 2 = 6 activities (was 3 + 3); weeks still sum to 13 each.
- **PhD thesis [A6a] added** to the Reference Documents list.
- Budget unchanged: **$157,924** (M1 $80,024 = 50.7%, M2 $77,900).
- Known minor imperfections that went in as-filed: M1 activity rows are in "train-first" display order (the "Activity 3 / Activity 5" cross-references point at the wrong rows); company name shows lowercase "zax analytics" (likely the DIP account legal-name value).

## What was submitted

- **Thesis:** a learned cross-modal fusion model — self-supervised via AIS-on co-occurrence to re-identify AIS-off ("dark") vessels across AIS, SAR, EO, acoustic, RF, and text.
- **TRL:** 2 → 3 (current = model formulated with built component evidence; end-state = demonstrated proof-of-concept incl. a live system at planetar.ca).
- **Budget:** $157,924 (≈63% of the $250K cap). $140/hr fully-loaded × 925 hrs labour + $7,924 MacBook Pro (Materials, M1). Two DIP milestones — Milestone 1 = $80,024 (50.7%, ≤70%), Milestone 2 = $77,900.
- **Paste-ready field content:** `submission/` fields 01–28.

## Open obligation

- **planetar.ca must be live and operable by evaluation** — cited as evaluator-drivable in the synopsis, overview, MC-1, and PRC-4. Load-bearing.

## Post-submission archive checklist (per `SUBMISSION-RUNBOOK.md` §5)

- [ ] Save the confirmation/Submitted-status screenshot (print-to-PDF: URL + timestamp).
- [x] **DIP acknowledgement email received 2026-06-01** — PSPC IDEaS team (tpsgc.paidees-apideas.pwgsc@tpsgc-pwgsc.gc.ca) confirming **CP6-132484**; CC'd DND.IDEaS + DND procurement-innovation; "the Program will contact you once the evaluation process is completed." Do-not-reply.
- [x] **Tagged + pushed** cited repos `submission-2026-05-30` (2026-05-31). Frozen commits the bid references, pushed to github.com/sness23: planetar-broker `8c7b936` · zmesg `c4faccd` · planetar-ontology `b642c49` · planetar-ui `24aa109` · planetar-ais `27309ca` · planetar-sat `d85d3d2` · planetar-eo `b867bc4` · planetar-acoustic `0fb09e4` · planetar-registry `c55ffb9`. doibio `7ff30af` tagged **locally only** (its remote `doibio2` is private). All 9 public tags are on GitHub for the 6-year audit window.
- [ ] Download the USPTO PDF for US 10,936,582.
- [ ] Two older TRL-3 drafts in DIP are abandoned — delete to avoid confusion (optional).

## What's next

- Evaluation typically **3–6 months** post-close; pre-qualified proposals enter a **180-day pool**.
- Hands-off the proposal until then; available only for DIP-portal-initiated clarifications.
