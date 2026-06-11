# PRC-5 — GBA Plus

> **Field cap:** 3,000 characters.
> **Score:** 5 pts. Done = 5, planned = 2, none = 0. Applied to the **technical solution**, not to the company.

---

## Draft (workspace markdown — strip headings before submission)

GBA Plus is applied to planetar's technical solution along three axes: (i) operator-side accessibility and multilingual usability, (ii) training-data and detection-output bias auditing, and (iii) deployment-side equity considerations.

**(i) Operator-side accessibility and multilingual usability — planned M5 deliverables.** CAF operators span genders, ages, abilities, neurodivergence, and linguistic backgrounds. The shell (`planetar-ui`, built on `sales4`'s React 19 base) will be developed against a documented accessibility checklist: keyboard navigation, ARIA roles on viewer panels and bus-message renderers, screen-reader compatibility, and a colour-vision-deficient (CVD)-safe palette throughout (no red/green-only encoding of confidence; double-encoded with shape and position). The map and entity-card viewers will adopt CVD-safe palettes; the waveform viewer will provide both visual and audio playback so colour-blind, low-vision, and audio-first analysts have equivalent evidence access. Bilingual English / French operation will be implemented via i18n in M5; translation surfaces are markdown-based and reviewable. The M5 accessibility checklist is itself a 1a deliverable.

**(ii) Training-data and detection-output bias auditing.** Every dataset used for training or evaluation will be audited for population-representation bias along axes relevant to the maritime domain. Specifically: AIS coverage is denser in commercial shipping lanes than in Indigenous coastal fishing waters, which can encode a "default" of large-vessel patterns and degrade detection on small Indigenous-fleet craft; SAR datasets (xView3, SAR-Ship, FUSAR) are weighted toward Northern Hemisphere traffic; surface EO datasets (Singapore Maritime, MODS, SeaShips) over-represent equatorial shipping; hydrophone archives skew toward cetacean-research deployments off particular coastlines. The 1a deliverables include a dataset bias-audit report quantifying representation gaps and an evaluation protocol that stratifies model performance by vessel class, operator type, and geographic region — a measurable artefact, not a generality.

**(iii) Deployment-side equity considerations.** The architecture's SWaP profile (commodity-Linux laptop-class hardware, no kernel bypass, no FPGA) means the system can deploy in low-bandwidth or remote settings — including Indigenous coastal communities, small-port operations, and Arctic coast-guard auxiliary stations — at the same fidelity as it deploys in central operations centres. Equity is a structural property of the architecture, not a deployment afterthought.

**Implementation in the 1a:** the dataset bias-audit report (M3–M4), the operator-accessibility checklist for `planetar-ui` (M5), and the multilingual-readiness verification (M5) are explicit deliverables in the work plan. GBA Plus analysis is applied to the technical solution through these deliverables, not deferred to a follow-on phase.

---

## Char-count budget

Target: ≤ 2,950 plaintext chars.

## Cross-references

- Operator-shell: `03-ARCHITECTURE.md` Layer 5.
- Dataset audit: `05-DATASETS.md`.
- SWaP claim: `03-ARCHITECTURE.md` Layer 2 + `01-CHALLENGE.md` desired outcomes table row 5.
