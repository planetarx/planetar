# Field 09 — MC-1: R&D activities to bring solution to stated TRL

**Form section:** Component 1a → MC-1
**Cap:** 3,000 characters
**Source:** `proposal/MC-1_trl.md` — RE-CENTERED 2026-05-30 (TRL-2 → TRL-3); blank lines stripped per DIP rule.
**Status:** ✅ READY — Field 08 (current TRL) = **2**. **LOCAL CHAR COUNT: 2,462 / 3,000.** Flowing prose.

## Paste-protocol notes

- Blank lines stripped (count toward DIP cap). Single newline between paragraphs.
- Flowing prose; describes the R&D that brings the solution (the learned fusion model) to its current TRL-2 — concept formulated with strong component evidence.

--- PASTE THIS BELOW ---
Current TRL: 2. The solution is a learned, self-supervised cross-modal fusion model that re-identifies a vessel after it disables its AIS transponder. The concept is fully formulated — a concrete architecture with substantial component-level evidence from the research and development below — but the integrated model is not yet trained or demonstrated; building and demonstrating it is the work the project funds, advancing the solution to TRL 3.
Per-modality ingest and detection are already built and open-source (about 6,000 lines, broker-integrated): a live AIS service, a Sentinel-1 synthetic-aperture-radar detector validated on a 433-megapixel scene, an electro-optical detector, and a passive-hydrophone detector. These are the encoders the fusion model consumes, so the perception front-end already exists rather than being assumed.
Entity resolution with provenance is likewise built: a zero-dependency production service, with 30 automated tests passing, resolves cross-modal observations to canonical vessel identities while preserving full lineage. It builds on an eighteen-month reference implementation and is a substantial development beyond the integrated-entity-view architecture of US Patent 10,936,582, on which the applicant is a named inventor (Salesforce-assigned), not the patent holder.
The deep-learning approach the model rests on is evidenced by the applicant's peer-reviewed work in the same regime it will operate in — scarce labels over continuous, unlabelled streams: semi-supervised deep learning at archive scale on hydrophone data (ORCA-SLANG, Interspeech 2021; and Sattar et al., IEEE PacRim 2011, evaluated on Ocean Networks Canada data at 95% accuracy). The planned self-supervised, joint-embedding design follows the established I-JEPA family of methods.
Underpinning these, a real-time, provenance-tracked substrate — a message bus, a zero-copy typed envelope, and a multi-pane analyst console — already runs as a live system and gives every inference an inspectable causal evidence chain with bit-exact replay.
Together, this work formulates the concept and de-risks it: the architecture, the encoders, the supervision signal, and the deployment surface all exist. The six months is therefore spent on the novel model itself rather than on plumbing — building, training, and calibrating it, and demonstrating it end-to-end on public data and as a live system at planetar.ca — advancing the solution from TRL 2 to TRL 3.
--- END PASTE ---
