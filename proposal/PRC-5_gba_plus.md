# PRC-5 — GBA Plus

> **Field cap:** 3,000 characters.
> **Score:** 5 pts. Done = 5, planned = 2, none = 0. Applied to the **technical solution**, not to the company.

---

## Draft (workspace markdown — strip headings before submission)

GBA Plus is applied to Planetar's technical solution along three axes: (i) operator-side accessibility and multilingual usability, (ii) training-data and fusion-model-output bias auditing, and (iii) deployment-side equity considerations. A cross-cutting principle: a human analyst stays accountable in the loop — the fusion model proposes, the operator adjudicates — guarding against automation bias that can fall unevenly on under-represented groups (small Indigenous-fleet craft, low-traffic regions).

**(i) Operator-side accessibility and multilingual usability — planned M5 deliverables.** CAF operators span genders, ages, abilities, neurodivergence, and linguistic backgrounds. The shipped shell (`planetar-ui`, React 19 + WS bridge, predecessor `sales4`) will be enhanced in M5 against a documented accessibility checklist: keyboard navigation, ARIA roles on viewer panels and bus-message renderers, screen-reader compatibility, and a colour-vision-deficient-safe palette (confidence double-encoded with shape, not colour alone); the waveform viewer offers visual and audio playback so low-vision and audio-first analysts have equivalent access. Bilingual English / French via i18n in M5. The M5 accessibility checklist is itself a 1a deliverable.

**(ii) Training-data and fusion-model-output bias auditing.** Every dataset used for training or evaluation will be audited for population-representation bias along axes relevant to the maritime domain. Specifically: AIS coverage is denser in commercial shipping lanes than in Indigenous coastal fishing waters, which can encode a "default" of large-vessel patterns and degrade detection on small Indigenous-fleet craft; SAR datasets (xView3, SAR-Ship, FUSAR) are weighted toward Northern Hemisphere traffic; surface EO datasets (Singapore Maritime, MODS, SeaShips) over-represent equatorial shipping; hydrophone archives skew toward cetacean-research deployments off particular coastlines. The 1a deliverables include a dataset bias-audit report quantifying representation gaps and an evaluation protocol that stratifies model performance by vessel class, operator type, and geographic region — a measurable artefact, not a generality.

**(iii) Deployment-side equity considerations.** The architecture's SWaP profile (commodity-Linux laptop-class hardware, no kernel bypass, no FPGA) means the system can deploy in low-bandwidth or remote settings — including Indigenous coastal communities, small-port operations, and Arctic coast-guard auxiliary stations — at the same fidelity as it deploys in central operations centres. Equity is a structural property of the architecture, not a deployment afterthought.

**Implementation in the 1a:** the dataset bias-audit report (M3–M4), the operator-accessibility checklist (M5), and multilingual-readiness verification (M5) are explicit work-plan deliverables — GBA Plus applied to the technical solution, not deferred to a follow-on phase.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars.

## Cross-references

- Operator-shell: `03-ARCHITECTURE.md` Layer 5.
- Dataset audit: `05-DATASETS.md`.
- SWaP claim: `03-ARCHITECTURE.md` Layer 2 + `01-CHALLENGE.md` desired outcomes table row 5.
