# Field 17 — PRC-5: Gender-based Analysis Plus (GBA Plus)

**Form section:** Component 1a → PRC-5
**Cap:** 3000 characters
**LOCAL CHAR COUNT:** 2894 (cap 3000)
**Source:** `proposal/PRC-5_gba_plus.md` — RE-CENTERED 2026-05-30 (learned-fusion-model thesis); blank lines stripped per DIP rule.
**Status:** ✅ READY (re-centered)

## Paste-protocol notes

- Blank lines stripped (they count toward the DIP cap). Single newline between paragraphs.
- Markdown emphasis removed; (a)/(b)/(c) and (i)/(ii) labels kept as plain text.
- After paste, DIP's counter should read ~2918 (±2 for newline encoding).

--- PASTE THIS BELOW ---
GBA Plus is applied to Planetar's technical solution along three axes: (i) operator-side accessibility and multilingual usability, (ii) training-data and fusion-model-output bias auditing, and (iii) deployment-side equity considerations. A cross-cutting principle: a human analyst stays accountable in the loop — the fusion model proposes, the operator adjudicates — guarding against automation bias that can fall unevenly on under-represented groups (small Indigenous-fleet craft, low-traffic regions).
(i) Operator-side accessibility and multilingual usability — planned Milestone 2 deliverables. CAF operators span genders, ages, abilities, neurodivergence, and linguistic backgrounds. The shipped analyst shell (planetar-ui, React 19) will be enhanced in Milestone 2 against a documented accessibility checklist: keyboard navigation, ARIA roles on viewer panels and bus-message renderers, screen-reader compatibility, and a colour-vision-deficient-safe palette (confidence double-encoded with shape, not colour alone); the waveform viewer offers visual and audio playback so low-vision and audio-first analysts have equivalent access. Bilingual English / French via i18n in Milestone 2. The accessibility checklist is itself a 1a deliverable.
(ii) Training-data and fusion-model-output bias auditing. Every dataset used for training or evaluation will be audited for population-representation bias along axes relevant to the maritime domain. Specifically: AIS coverage is denser in commercial shipping lanes than in Indigenous coastal fishing waters, which can encode a "default" of large-vessel patterns and degrade detection on small Indigenous-fleet craft; SAR datasets (xView3, SAR-Ship, FUSAR) are weighted toward Northern Hemisphere traffic; surface EO datasets (Singapore Maritime, MODS, SeaShips) over-represent equatorial shipping; hydrophone archives skew toward cetacean-research deployments off particular coastlines. The 1a deliverables include a dataset bias-audit report quantifying representation gaps and an evaluation protocol that stratifies model performance by vessel class, operator type, and geographic region — a measurable artefact, not a generality.
(iii) Deployment-side equity considerations. The architecture's SWaP profile (commodity-Linux laptop-class hardware, no kernel bypass, no FPGA) means the system can deploy in low-bandwidth or remote settings — including Indigenous coastal communities, small-port operations, and Arctic coast-guard auxiliary stations — at the same fidelity as it deploys in central operations centres. Equity is a structural property of the architecture, not a deployment afterthought.
Implementation in the 1a: the dataset bias-audit report, the operator-accessibility checklist, and multilingual-readiness verification are explicit Milestone 2 work-plan deliverables — GBA Plus applied to the technical solution, not deferred to a follow-on phase.
--- END PASTE ---
