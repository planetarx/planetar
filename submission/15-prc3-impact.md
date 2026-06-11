# Field 15 — PRC-3: Impact of Proposed Solution

**Form section:** Component 1a → PRC-3
**Cap:** 3000 characters
**LOCAL CHAR COUNT:** 2494 (cap 3000)
**Source:** `proposal/PRC-3_impact.md` — RE-CENTERED 2026-05-30 (learned-fusion-model thesis); blank lines stripped per DIP rule.
**Status:** ✅ READY (re-centered)

## Paste-protocol notes

- Blank lines stripped (they count toward the DIP cap). Single newline between paragraphs.
- Markdown emphasis removed; (a)/(b)/(c) and (i)/(ii) labels kept as plain text.
- After paste, DIP's counter should read ~2494 (±2 for newline encoding). Inline-labelled prose (no bullet markers) so (b)/(c) render as paragraphs, not a run-on dash list.

--- PASTE THIS BELOW ---
(a) Addresses a stated capability gap. The Challenge names the gap: siloed multi-domain streams, rule-based aggregation, opaque outputs. The AIS-off ("dark vessel") problem in the Maritime Task Group Operations example is operationally costly — illegal fishing, sanctions evasion, ship-to-ship transfers, and Arctic sovereignty incursions all turn on AIS being disabled. Today this is handled with stovepiped per-modality pipelines and post-hoc explanation. Planetar replaces them with one learned cross-modal model that resolves identity from the modalities that persist after the beacon goes dark, and surfaces calibrated, explainable outputs. By restoring track continuity on adversaries that deliberately evade monitoring it reduces a surveillance vulnerability; by sharing the decision between AI correlation and analyst adjudication it increases speed of decision-making — the Challenge's two stated benefits. The capability is built on the applicant's own open-source code and new, Canadian-owned IP, and is open-source-replicable on commodity Linux — a sovereign capability interoperable with allies, aligned to the DND/CAF AI Strategy and Force Capability Plan priorities for ISR and C2.
(b) Enhances S&T capability. Self-supervised cross-modal fusion in defence ISR: using an intermittent strong identifier (AIS) to self-supervise learned association across SAR/EO/acoustic/RF/text is a capability not present in current maritime ISR. Calibrated, explainable identity: propagated uncertainty plus an attention-grounded causal chain make outputs accreditable, addressing a known barrier to operational AI adoption. Methodological transfer: the applicant's archive-scale semi-supervised acoustic ML extends from cetacean-call to vessel-signature discrimination. Productionized entity resolution: retyped from research-entity to vessel-entity context, with full provenance on every resolved identity.
(c) Matures the field. A generalizable pattern: "an intermittent strong identifier self-supervises the rest" transfers to Arctic, airborne, and land / edge multi-domain settings, and the applicant intends to publish it as a contribution to CAF multi-domain ISR. Lower barrier to accreditable AI: calibrated, lineage-bearing outputs make explainability a system property rather than an afterthought, supporting DND/CAF AI Strategy and ATO pathways. Durable output: applicant-controlled code and new, Canadian-owned IP outlast the contract window as papers and reference implementations.
--- END PASTE ---
