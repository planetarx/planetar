# Re-center: the learned cross-modal fusion model is the thesis

**Decision (2026-05-30, user):** full re-center toward CH13's actual demand — *an AI model that learns to fuse* — and add **text** + promote **RF** to real. The nanosecond bus stops being "the thesis" and becomes enabling infrastructure. This file is the SSOT all narratives must match.

## Why (alignment verdict)

CH13 repeats one demand: *"an AI model that can … fuse,"* *"learned, adaptive fusion … rather than static aggregation,"* *"advanced deep learning architectures for spatiotemporal alignment, uncertainty propagation."* The prior draft led with a message bus and a heuristic (log-score + threshold) fusion — which reads to an ML reviewer as exactly the "rule-based / static aggregation" the challenge rejects. Estimated at-risk score with an AI reviewer: **below the 70 threshold.** The fix is to re-rank: make the *learned* model the thesis; demote the bus to the enabler that makes the model deployable, explainable, replayable.

## New thesis (one sentence)

> **planetar is a learned cross-modal fusion model for maritime domain awareness** that encodes six heterogeneous streams into a shared spatiotemporal embedding where one vessel's observations resolve to a single calibrated, uncertainty-scored, explainable identity — **even when its AIS beacon is dark** — with a human analyst in the loop and adaptive edge perception. A working real-time, provenance-tracked substrate the applicant has already built makes the model deployable and auditable; **the science of the 1a is the fusion model itself.**

## The model (proposed, TRL 2 → 3)

The central R&D contribution, and the novel idea:

1. **Per-modality encoders → shared embedding.** Each modality has an encoder (SAR chip CNN, EO chip CNN, acoustic spectrogram/CAR-FAC→CNN, AIS kinematic+text sequence encoder, RF emission-signature encoder, text/report transformer) projected into one metric space [I1, I2].
2. **Self-supervision from AIS co-occurrence — the key insight.** During **AIS-on** periods, a broadcasting vessel's identity *labels for free* the concurrent SAR/EO/acoustic/RF/text observations in its space-time neighborhood. The model learns the cross-modal association from this signal (contrastive / joint-embedding, I-JEPA-family [I6] — the lineage of the applicant's v1 JEPA design). At inference it applies that learned association to **re-identify AIS-off (dark) vessels.** This dissolves the scarce-label barrier that blocks supervised maritime fusion, and it is, to our knowledge, novel.
3. **Learned association head.** A transformer over the *set* of concurrent observations [I3] groups observations into vessel identities (set→identity), the analog of metric-learning re-ID [I4].
4. **Uncertainty propagation.** Per-modality evidential heads [I5] propagated through fusion; the fused identity score is **conformally calibrated** [E1, E2] for distribution-free coverage. (Answers "uncertainty propagation, confidence scoring.")
5. **Native explainability.** Attention weights over modalities/observations + the causation-chain provenance → the analyst sees *which* evidence drove the identity and *how sure* — operator trust + accreditation.
6. **Adaptive, constraint-aware perception.** An agentic controller rewrites the edge perception graph (MediaPipe `.pbtxt` [H1]) per SWaP/threat — realizing "learned, **adaptive** fusion … rather than **static aggregation**."
7. **Human-in-the-loop (CI).** Analyst adjudication is a first-class input that corrects the graph and feeds an active-learning loop — the applicant's shipped Orchive pattern [A6a][A9].

The applicant's **peer-reviewed semi-supervised deep learning at archive scale on unlabeled acoustic streams** (ORCA-SLANG [A2]; Sattar 2011, 95% on ONC data [A1]) is the direct evidence this learning approach is achievable by this applicant.

## Modalities — now six, mirroring the challenge's "sensor, text, RF"

| Modality | Status | Role |
|---|---|---|
| AIS (kinematic + text fields) | built ingest | supervision signal + dark-event trigger |
| SAR (Sentinel-1) | built detector → encoder | imagery |
| EO | built detector → encoder | imagery/video |
| Acoustic (hydrophone) | built detector → encoder | audio |
| **RF emissions** | **promoted stub → real (proposed)** | RF/SIGINT |
| **Text (reports / notices-to-mariners / OSINT)** | **new (proposed)** | text intelligence |

## Build vs. propose (honesty spine — re-ranked, unchanged in rigor)

- **BUILT — the enabler (TRL 3):** real-time provenance substrate (bus), typed envelope, entity-graph / identity-resolution service (`planetar-ontology`, patent + doibio), analyst shell, four per-modality ingest+detectors. *These de-risk the model; they are not the thesis.*
- **PROPOSED — the science (TRL 2 → 3):** the learned cross-modal fusion model (encoders → shared embedding → association head → uncertainty → explanation), the **text** + **RF** modalities and their encoders, conformal calibration on the fused model, adaptive MediaPipe perception, the CI active-learning loop.

## Doctrine hooks to thread (a DND reviewer rewards these)

CAF Digital Campaign Plan · DND/CAF AI Strategy · Force Capability Plan (ISR, C2, operational resilience) · interoperability with allies (NATO/5-Eyes MDA) · secure operations in contested/degraded environments · accreditation / ATO readiness · classification levels (Protected B at 1a; multi-level design path).

## Cut / demote from the SCORED narratives

- Nanosecond latency headline → at most one SWaP/edge line. **Not** the thesis.
- Bus internals (CRC32 WAL, lock-free CAS, memfd/SCM_RIGHTS, p99 µs/TCP numbers) → out of scored narratives (keep in `03-ARCHITECTURE` appendix).
- "Slack/Discord/Quip/Palantir 4-pane" branding → reframe shell as the explainable human-in-the-loop decision surface (CI).
- `planetar-registry` codegen → drop from narratives.

## Per-narrative re-center checklist

- **MC-1 (TRL):** TRL 3 for the substrate + detectors; **fusion model TRL 2 → 3** is the 1a R&D. State the build/propose split plainly. *(done this pass: pending)*
- **MC-2 (alignment):** model-first solution; 6 types incl. text+RF; self-supervised; uncertainty+conformal; explainable/operator-trust; doctrine; essential-outcome cleared. ✅ this pass
- **PRC-1 (S&T merit):** lead evidence/SOTA with multimodal-fusion + semi-supervised-acoustic + uncertainty; demote messaging to one SWaP line.
- **PRC-2 (novelty):** self-supervised AIS→dark-vessel re-ID as headline; uncertainty-propagating calibrated fusion; adaptive perception. ✅ this pass
- **PRC-3 (impact):** matures *AI multi-domain fusion*; reduces vulnerabilities; decision speed; sovereign Canadian-IP AI capability.
- **PRC-4 (feasibility):** built substrate de-risks; model is scoped TRL 2→3 on public data; self-supervision removes label barrier; milestones re-pointed to the model.
- **PRC-5 (GBA+):** keep (CI + bias audit + accessibility); tie to human-in-the-loop accountability.
- **PRC-6 (desired outcomes):** outcome 1 = the model's alignment+uncertainty; 2 = entity graph; 3 = provenance/classification; 4 = explainable + CI; 5 = SWaP + MediaPipe.
- **PRC-7 (budget):** re-point M3/M4 to the learned model + text/RF encoders; verify SC-1 still passes.
- **Supporting:** `02-STRATEGY`, `03-ARCHITECTURE`, `05-DATASETS` (add text+RF datasets), `06-REFERENCES` (I-group ✅), `01-CHALLENGE`, `04-PORTFOLIO`, `README`, matrix.
