# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

> **Read the workspace-level `../CLAUDE.md` first.** It is the authoritative source for the broader `planetarx/` workspace: the canonical code spine (external repos `planetar-broker`, `zmesg`, `planetar-ui`, `planetar-ais`, `planetar-sat/-eo/-acoustic`, `planetar-ontology`, `planetar-registry`, `doibio`), the load-bearing proposal facts, the v1 (`zdefence/`) relationship, and the working norms (word budgets, provenance-over-polish, conservative benchmark claims, patent framing, absolute dates). Everything below is **specific to this `planetar/` git repo** and does not repeat that file.

## What this repo is

`planetar/` is the proposal-authoring git repo (`git@github.com:sness23/planetar.git`, branch `main`) for the **IDEaS CFP6 Challenge 13, Component 1a** submission. It contains **only Markdown** — no source code, no build system, no tests, no package manager. "Working in this repo" means editing prose against hard character caps and keeping every claim traceable.

Note: the parent `planetarx/` directory is *not* a git repo, but **this `planetar/` subdirectory is** — commit here.

## Current state: SUBMITTED (post-submission posture)

**The bid was first submitted 2026-05-30 (CP6-132296) and then REPLACED 2026-06-01 with a revised version, DIP ref `CP6-132484` (TRL 2→3, $157,924) — the replacement is the authoritative, evaluated submission.** `docs/submission-record.md` is the authoritative current-state file — read it before assuming what is or isn't done. Both filed-summary PDFs are archived in the repo root.

Consequences for any work now:
- **`README.md` is stale** — it is the *pre-submission* dashboard (dated 2026-05-28, "W5", "the bus is the product"). Treat it as historical. `docs/submission-record.md` + the auto-memory supersede it.
- The submitted content lives frozen in `submission/` fields 01–28. Don't silently "improve" submitted text as if it were still a draft — the artifact is filed.
- The one live obligation: **planetar.ca must stay up and evaluator-drivable** (cited in the synopsis, overview, MC-1, PRC-4). Evaluation runs ~3–6 months.

## The three-layer document pipeline (the thing to understand)

Content flows through three representations. Editing the wrong layer is the most common mistake.

1. **Workspace thinking docs** — `01-CHALLENGE.md` … `08-OPEN-QUESTIONS.md`, `MOAT-STRATEGY.md`. The rubric, strategy, 5-layer architecture, datasets, references, timeline, and the open-question/audit log (Q1–Q13). Reasoning lives here.
2. **`proposal/`** — the 9 scored narrative *drafts* (`MC-1`, `MC-2`, `PRC-1`…`PRC-7`). Authoring format: a `## Draft` body plus a `## Char-count budget` section. This is where you edit prose.
3. **`submission/`** — paste-ready DIP portal files (01–28), one per wizard form field **in portal order**. Each has a `--- PASTE THIS BELOW ---` / `--- END PASTE ---` block, a `LOCAL CHAR COUNT:`, and **blank lines stripped**. Derived from `proposal/`. `submission/README.md` is the field-by-field portal map.

`submission/` is downstream of `proposal/`. A change to scored content means: edit the `proposal/` draft → re-strip → update the matching `submission/NN-*.md` paste block → re-check the count. Files `20`–`28` (work plan, milestones, financials, location, glossary, references, certifications) exist only in `submission/` (later wizard steps, no `proposal/` source).

## The thesis re-center (2026-05-30) — SSOT

`docs/recenter-learned-fusion.md` is the **single source of truth all narratives must match.** The thesis is a **learned cross-modal fusion model** (self-supervised on AIS-on co-occurrence to re-identify dark/AIS-off vessels across AIS, SAR, EO, acoustic, RF, text). The nanosecond bus is **enabling infrastructure, not the thesis** — keep bus internals (CRC32 WAL, lock-free CAS, p99/TCP latency numbers) out of the *scored* narratives; they belong in the `03-ARCHITECTURE.md` appendix. The deleted `submission/*-v2.md` files were the old bus-first framing — don't resurrect that framing.

`docs/cfp6-alignment-matrix.md` is the clause-by-clause traceability source for `MC-2_alignment.md` and `PRC-6_desired_outcomes.md`; keep all three consistent.

## The character-count protocol (this repo's "build")

There is nothing to compile; the equivalent discipline is staying under DIP's hard caps. **Blank lines between paragraphs count toward the cap** — paste blocks separate paragraphs with a single newline. Caps: Project Synopsis **2,000**, scored narratives & Project Overview **3,000**.

- Count characters (not words): `wc -m <file>` on the stripped paste block, or count between the `PASTE THIS` markers. The submission file's `LOCAL CHAR COUNT:` must match DIP's counter within ±2.
- Tightest field is the **Project Synopsis (~36 chars headroom)**; don't add blank lines back.
- Smart-quote / em-dash auto-conversion in DIP can push a file over cap — replace `" " ' ' — –` with ASCII `" ' -` if DIP disagrees with the local count.
- Markdown→plaintext stripping (bold/italic/headings/workspace meta) before pasting is documented step-by-step in `docs/pre-submission-checklist.md` (T-3).

## Operating playbooks

- `docs/SUBMISSION-RUNBOOK.md` — top-to-bottom submission walkthrough (T-8 … T+4) + post-submission archive.
- `docs/pre-submission-checklist.md` — day-of protocol, char-strip steps, Q9/Q10/Q13 sanity-checks.
- `docs/built-services-inventory.md` — audit map: every "built" claim → repo path + `wc -l`. Use this to verify a claim before repeating it.
- `docs/benchmark-2026-04-27.md` / `docs/compute-estimate.md` — the measured-latency and $18K-compute backing.

## Gotchas

- `.claude/worktrees/repoint-pt2/` is a stale git worktree, **not** the live tree — never edit files there.
- `submission/.#25-glossary.md` is an Emacs lock symlink (dangling) — ignore; don't commit or "fix" it.
- Repos the bid cites are frozen at tag `submission-2026-05-30` (commit hashes in `docs/submission-record.md`). If you re-verify a claim against external code, check out that tag, not `HEAD`.
