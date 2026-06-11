# MC-2 — Alignment with the S&T Challenge

> **Field cap:** 3,000 characters.
> **Pass/fail.** Must describe (i) the solution, (ii) its scientific/technological basis, (iii) how it meets each Essential Outcome.

---

## Draft (workspace markdown — strip headings before submission)

**Solution.** *planetar* is a multi-modal situational-awareness platform whose core choice is a single nanosecond message bus through which all observations, derived events, entity-graph updates, and analyst actions flow as typed envelopes. Heterogeneous data (AIS, Sentinel-1 SAR, surface EO, passive hydrophone, non-AIS RF) are ingested as bus messages, fused through a patent-backed entity-graph layer that resolves cross-modal observations to canonical vessel entities, and surfaced in a Slack-style multi-viewer shell whose map / timeline / entity-card / waveform / channel panels are bus consumers. The 1a flagship is cross-modal re-identification of AIS-off ("dark") vessels, aligning directly with CH13's *Maritime Task Group Operations* example.

**Scientific and technological basis.** Three lineages: **(i)** lock-free ultra-low-latency messaging — LMAX Disruptor lineage [B1a] extended in applicant's `planetar-broker` (predecessor `zbroker0`) with multi-transport fan-out, CRC32 WAL, zero-copy nanosecond envelopes (predecessor reproduced p50 = 80–140 ns / p99 = 400–900 ns end-to-end on commodity Linux, 1M-msg benchmark, 2026-04-27); **(ii)** cross-system entity resolution with provenance — US Patent 10,936,582 [A8a] and applicant's ~20 k-LOC reference implementation `doibio` (event-sourced append-only log; 630-line confidence-weighted identity-resolution engine; Salesforce-inspired Party model to be retyped for vessels in M4); **(iii)** semi-supervised deep learning on unlabeled acoustic archives — Sattar et al. PacRim 2011 (95 % accuracy on Ocean Networks Canada hydrophone data) [A1] and Bergler et al. ORCA-SLANG, Interspeech 2021 [A2]. Fusion uses log-score decomposition (prior + per-modality evidence + uncertainty + penalty) calibrated by conformal prediction; every output's `causation_id` references its inputs.

**Essential Outcome compliance.** The Essential Outcome requires fusion of at least two heterogeneous types producing classifications / detections / correlations. planetar fuses **four production modalities** plus a fifth stub topic: AIS broadcasts (→ position and dark-event detection); Sentinel-1 SAR (→ CFAR + chip classifier per xView3 [D1]); surface EO (→ fine-tuned detector on Singapore Maritime / MODS); passive hydrophone (→ semi-supervised classifier per ORCA-SLANG template; vessel-noise discrimination via ShipsEar / DeepShip [D4][D5]); non-AIS RF (stub). Outputs span detection, classification (vessel class), and correlation (cross-modal `vessel.v1.ReIDCandidate` entities with calibrated confidence and explicit per-modality evidence pointers). Fusion is learned and adaptive (deep semi-supervised classification + log-score adjudication), not rule-based aggregation. Explainability is structural: every detection is an envelope whose causation chain renders as click-through navigation from output back to raw input. Four-plus-one fused, three output classes, full lineage — the two-modality bar cleared with margin.

---

## Char-count budget

Target: ≤ 2,950 chars. Trim modality bullet list to compact prose if over.

## Cross-references to workspace

- Architecture lineages: `02-STRATEGY.md` + `03-ARCHITECTURE.md` Layers 1–5.
- Bus measurement: `03-ARCHITECTURE.md` Layer 2.
- Entity graph + patent: `03-ARCHITECTURE.md` Layer 4 + `04-PORTFOLIO.md` Tier 1B.
- Datasets: `05-DATASETS.md`.
- References: `06-REFERENCES.md` [A1, A2, A8a, B1a, D1, D4, D5].
