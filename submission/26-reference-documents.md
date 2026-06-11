Reference Documents. Each needs Title / Author / Publication Date / Relevance to the Work.
NOTE on dates: the form wants a specific date (e.g., Mar 2, 2021). Both patent grant dates are now confirmed (2021-03-02 and 2022-09-13); where marked VERIFY, confirm the exact date before entering — only xView3 and the PhD-thesis exact date still need checking; the rest are conference/publication dates.

=== 1 — entity-resolution background IP (named inventor) ===
Title: US Patent 10,936,582 B2 — Integrated entity view across distributed systems
Author: Steven R. Ness, one of 19 named inventors (assignee: salesforce.com, inc. — named-inventor credit only, not the assignee)
Publication Date: 2021-03-02 (grant date; confirmed on Google Patents / USPTO)
Relevance: The integrated-entity-view / entity-resolution architecture the project takes as a starting point and develops substantially beyond — the maritime entity graph that resolves cross-modal observations to canonical vessels with full provenance.

=== 1b — second entity-resolution-adjacent background patent (named inventor) ===
Title: US Patent 11,442,952 B2 — User interface for commerce architecture
Author: Steven R. Ness, one of 11 named inventors (assignee: Salesforce, Inc. — named-inventor credit only, not the assignee)
Publication Date: 2022-09-13 (grant date; confirmed on Google Patents; issued from App. 16/264,391)
Relevance: Canonical-data-model mapping with matching / reconciliation rules for unified profiles — identity-resolution prior art the applicant contributed to; secondary background credential, distinct from and superseded by the project's new fusion-model IP.

=== 2 — applicant's acoustic credential (Ocean Networks Canada data) ===
Title: Automatic Event Detection for Long-Term Monitoring of Hydrophone Data
Author: F. Sattar, P. Driessen, G. Tzanetakis, S. R. Ness, W. Page
Publication Date: Aug 23, 2011 (IEEE Pacific Rim Conference, PacRim 2011)
Relevance: Applicant's co-authored, peer-reviewed event detection on Ocean Networks Canada hydrophone data at 95% accuracy — direct precedent for the acoustic modality and for machine learning on continuous, scarcely-labelled streams.

=== 3 — applicant's semi-supervised deep-learning credential ===
Title: ORCA-SLANG: An Automatic Multi-Stage Semi-Supervised Deep Learning Framework for Large-Scale Killer Whale Call Type Identification
Author: C. Bergler, M. Schmitt, A. Maier, H. Symonds, P. Spong, S. R. Ness, G. Tzanetakis, E. Noeth
Publication Date: Aug 30, 2021 (Interspeech 2021)
Relevance: Applicant's co-authored semi-supervised deep learning at archive scale on unlabelled hydrophone streams — evidence the proposed self-supervised approach transfers to the scarce-label maritime regime.

=== 3b — applicant's PhD thesis (archive-scale bioacoustic ML + human-in-the-loop) ===
Title: The Orchive: A System for Semi-Automatic Annotation and Analysis of a Large Collection of Bioacoustic Recordings
Author: Steven R. Ness (PhD thesis, University of Victoria, Department of Computer Science; supervisor George Tzanetakis)
Publication Date: 2013 — VERIFY exact date on UVic DSpace if the form requires a specific day
Relevance: The applicant's doctoral work — archive-scale semi-supervised machine learning on a 20,000-hour, 30-year OrcaLab hydrophone archive, with a human-in-the-loop design in which expert and citizen-science annotations train classifiers whose outputs are shown back in the same interface. Establishes both the scarce-label bioacoustic-ML pedigree behind the fusion model and the collaborative-intelligence / operator-in-the-loop pattern the analyst surface adopts.

=== 4 — the public dark-vessel benchmark ===
Title: xView3-SAR: Detecting Dark Vessels in Synthetic-Aperture Radar Imagery
Author: Defense Innovation Unit (DIU), U.S. Department of Defense, et al.
Publication Date: 2021 — VERIFY exact date
Relevance: Public, DoD-component-funded dark-vessel-detection dataset (Sentinel-1 SAR co-located with AIS) — the supervised training anchor and the public benchmark used to evaluate the model.

=== 5 — the cross-modal embedding method ===
Title: Learning Transferable Visual Models From Natural Language Supervision (CLIP)
Author: A. Radford, J. W. Kim, C. Hallacy, et al. (OpenAI)
Publication Date: Feb 26, 2021
Relevance: The contrastive shared-embedding method the cross-modal fusion model builds on, projecting heterogeneous modalities into one common space.

=== 6 — the self-supervised architecture family ===
Title: Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture (I-JEPA)
Author: M. Assran, Q. Duval, I. Misra, et al. (Meta AI)
Publication Date: Jan 19, 2023
Relevance: The joint-embedding self-supervised architecture family behind the AIS-co-occurrence training signal (using AIS-on periods to label concurrent cross-modal observations).

=== OPTIONAL (add if you want fuller technical grounding) ===

Title: Attention Is All You Need
Author: A. Vaswani, N. Shazeer, N. Parmar, et al.
Publication Date: Jun 12, 2017
Relevance: The transformer/attention architecture used for the fusion association head over concurrent observations.

Title: A Gentle Introduction to Conformal Prediction and Distribution-Free Uncertainty Quantification
Author: A. N. Angelopoulos, S. Bates
Publication Date: Jul 15, 2021
Relevance: The conformal-prediction method giving distribution-free confidence bounds on the model's calibrated outputs.

Note: the full citation list is in 06-REFERENCES.md if you want to add more, but these 6 (plus 2 optional) are the documents the work's basis genuinely depends on. Don't over-stuff this section — critical documents only.
