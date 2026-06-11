# 07 — Timeline

Two calendars here: (A) **proposal calendar** — from today to 2026-06-02 submission — and (B) **execution calendar** — the 6-month 1a contract once awarded.

---

## (A) Proposal calendar — 12 days to deadline (~8 to target submission)

Today: **2026-05-21** (mid-**W4**). Hard deadline: **2026-06-02 14:00 EDT**. Target submission: **2026-05-29** (3 working days of buffer before the deadline, in case the portal flakes).

| Week | Dates | Milestones |
|---|---|---|
| **W0** | 2026-04-24 – 2026-04-26 | ✅ Directory created. STRATEGY, ARCHITECTURE, TIMELINE, OPEN-QUESTIONS v0 drafted. 110 ns reproduced on this box. |
| **W1** | 2026-04-27 – 2026-05-03 | ✅ 01-CHALLENGE, 04-PORTFOLIO, 05-DATASETS, 06-REFERENCES drafted. ✅ TRL claim (updated 2026-05-30: solution = the learned fusion model, TRL 2 → 3; built substrate/components are TRL 3–4 enabling infra). ✅ MC-1, MC-2 drafted. |
| **W2** | 2026-05-04 – 2026-05-10 | ✅ PRC-1, PRC-2, PRC-3, PRC-4 drafted (delivered 2026-04-27, ahead of schedule). ✅ 1M-message reproducible benchmark + appendix (`docs/benchmark-2026-04-27.md`) done. ✅ Cold red-team pass complete; critical bugs fixed (p99 numbers, patent framing, GBA+ aspirational claims, "fuses five" overshoot). |
| **W3** | 2026-05-11 – 2026-05-17 | ✅ PRC-5, PRC-6, PRC-7 drafted. ✅ Q9 patent-language audit applied (6 edits). ✅ Q10 CRANK citation count verified (148). ✅ Q13 citation-byline sweep [A1–A8b]: A2 byline fixed, A6 thesis/arXiv split. ✅ 6 `planetar-*` repos + `zmesg` open-sourced (gitleaks-scanned clean). ✅ TCP perf baseline measured 2026-05-14 (paced 200k msg / p50 34 µs / p99 424 µs). ✅ planetar-ontology P1–P5 done (Node/TS, 30 tests pass). Remaining: lock proposer rate × hours; verify A8b on USPTO; full doibio cleanup into doibio3. |
| **W4** | 2026-05-18 – 2026-05-24 | ⏳ Current week (day 4 of 7). Red-team pass. Have at least one technical reader (unrelated to this project) read the narrative cold and flag unclear claims. Rewrite. Build the demo screenshots / architecture diagrams that will go into the submission package. Finalize budget with CRA-compatible categories. **DIP-registration approval must be re-confirmed by end of week** or fallback submission path triggers. |
| **W5** | 2026-05-25 – 2026-05-29 | Buffer week. **Submit by end of day 2026-05-29.** Do not touch the proposal after submission. |
| (slack) | 2026-05-30 – 2026-06-02 | Emergency slack for portal issues, amendments, clarifications. |

### Week-over-week exit criteria

- **End of W1:** every top-level `planetar/` doc has content, TRL claim is locked, a single "story spine" paragraph is written that every narrative will point back to.
- **End of W2:** four narratives drafted to at least 80% length. Benchmark appendix done.
- **End of W3:** all nine (MC-1, MC-2, PRC-1..7) narratives drafted.
- **End of W4:** one cold external read complete. All narratives within character limits. Budget final.
- **End of W5:** submitted.

### Kill criteria (when to NOT submit)

- If by end of W2 the benchmark doesn't reproduce ≤200 ns p50 on a fresh boot. Claim downgrades, and we have to decide whether the downgraded number still defends option A scope.
- If by end of W3 we discover the sales4 shell can't be cleanly made to subscribe to bus topics through a gateway. Scope falls back to option B (one modality end-to-end, rest stubbed).
- If DIP registration is not approved by end of W4. Then we have to use the fallback submission path.

---

## (B) Execution calendar — 6 months if awarded

Assumes award announcement ~2026-08 and kick-off ~2026-09. Calendar here is relative (M1–M6) to avoid guessing the exact start.

### M1 — Bus hardening

- Harden `planetar-broker` (the new C broker that supersedes the predecessor `zbroker0` under Zax Analytics): tighten the Makefile, add unit tests, add a containerized single-command dev loop.
- Formalize the envelope into a versioned protobuf `planetar.envelope.v1` on top of the existing zmesg binary wire format. Publish the `.proto`.
- Stand up CI on a reference Linux box. Publish a reproducible SHM/TCP/UDP benchmark report for `planetar-broker` alongside the predecessor `docs/benchmark-2026-04-27.md`.
- **Deliverable:** tagged `planetar-broker v0.1.0`, fresh benchmark report, architecture doc.

### M2 — Ingress adapters + storage

- AIS ingress (MarineCadastre / AIS-Hub replay) → `ais.v1.Position`.
- SAR ingress (Sentinel-1 via Copernicus Open Access Hub) → `sar.v1.Tile`.
- EO ingress (Singapore Maritime Dataset + MODS) → `eo.v1.Frame`.
- Hydrophone ingress (Ocean Networks Canada public feeds) → `acoustic.v1.Frame`.
- WAL-backed cold storage layout finalized; replay-from-cursor verified.
- **Deliverable:** four ingress adapters, recorded datasets, replayable sessions.

### M3 — Detectors (per-modality)

- `ais.gap` — heuristic detector. Small, shippable in days.
- `sar.chip` — port a public CFAR + CNN chip classifier; tune on xView3-lite or Sentinel-1 public labels.
- `eo.chip` — fine-tune a public detector on Singapore Maritime.
- `acoustic.event` — apply ORCA-SLANG-style semi-supervised pipeline to public hydrophone archive (applicant's own prior methodology).
- **Deliverable:** five detector processes emitting typed bus messages with causation ids.

### M4 — Entity graph + cross-modal re-ID (the research)

- Embedded graph store; entity schema; observation edges with `msg_id` lineage.
- Cross-modal re-ID consumer: takes `ais.v1.Gap` + concurrent detections within spatial/temporal window and publishes `vessel.v1.ReIDCandidate` with `evidence: [msg_id]` and a log-score breakdown (prior + evidence + uncertainty + penalty — pattern from `crank3`).
- Calibration. Conformal prediction on the fused score so outputs are *calibrated*, not just ordered.
- **Deliverable:** re-ID candidate stream on the bus, graph state queryable, calibration report.

### M5 — Shell extensions (viewers)

- Map viewer. Live tracks from `ais.v1.Position` and detections.
- Timeline viewer. Event ribbon, scrub, filter.
- Entity-card viewer. Evidence list, provenance clicks-through, confidence.
- Waveform / chip viewer. SAR chip, EO crop, hydrophone spectrogram for analyst adjudication.
- Channel viewer is the existing sales4 chat, confirmed to subscribe to bus topics.
- **Deliverable:** `planetar-ui` running against the bus, reviewer-drivable in a browser.

### M6 — Demo, eval report, and the 1b pitch

- End-to-end synthetic scenario: vessel transits Salish Sea, disables AIS during a Sentinel-1 pass, is detected in SAR and on a hydrophone, re-identified, surfaced in viewers. Fully replayable from WAL.
- Evaluation on public benchmarks (xView3 for SAR, Singapore Maritime for EO, community metrics for AIS-gap and acoustic).
- Final report: architecture, measurements, evaluation, limitations, TRL advancement evidence.
- Component 1b proposal drafted, packaged for follow-on submission.
- **Deliverable:** demo recording, final report, 1b proposal package.

### Scope discipline (what is OUT of M1–M6)

- No classified data. No GFP. No OOR data. No DND personnel.
- No model training at frontier scale. All detectors use public baselines or the applicant's own prior published methodology.
- No production deployment. No PSPC-IT integration. No operator trials beyond the applicant.
- No multi-analyst shell (stays single-user for the 1a).
- No FPGA / kernel-bypass work. The measured SHM path is the claim.

### Risk register (keep short)

| Risk | Likelihood | Mitigation |
|---|---|---|
| SAR baseline is weaker than expected on arctic imagery | Medium | Drop to temperate waters for the demo; still satisfies essential outcome |
| Hydrophone public data sparse for vessel transits | Medium | Fall back to acoustic event detection generally; narrative keeps the modality without overclaiming vessel-id |
| Re-ID fusion underperforms on calibration | Low | We own conformal prediction in scope; calibration is a back-stop that doesn't require the fusion to be strong, only well-modeled |
| Shell extension takes longer than M5 allots | Medium | Map + timeline + entity-card are priority; waveform can ship as v0.1 (plain `<audio>` / `<img>`) |
| Solo-founder sickness / burnout | Medium | Buffer week in M6 is scope-cut insurance, not schedule-cut |
