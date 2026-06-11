# Field 03 — Project Synopsis

**Form section:** Component 1a → 2 - Project Description → A
**Field label:** `*Project Synopsis`
**Cap:** **2,000 characters**
**Type:** Text area + optional 1-page PDF/JPEG upload (do NOT upload — Field 05 acknowledges upload won't be evaluated)
**Status:** ✅ RE-CENTERED 2026-05-30 (learned-fusion-model thesis); publicly shareable per Field 04

## Audience

Published externally if funded (per Field 04). Written for non-CH13-domain readers — leads with the problem and the AI model, not the infrastructure. No internal jargon.

--- PASTE THIS BELOW ---
Dark vessels — vessels that disable their AIS transponders to evade tracking — are a central capability gap for maritime situational awareness. Illegal fishing, sanctions evasion, ship-to-ship transfers, and Arctic sovereignty incursions all depend on AIS going dark. Today this is handled with per-modality pipelines and explainability bolted on afterward.
Planetar is a learned cross-modal AI model that closes this gap. It encodes six streams — AIS, synthetic-aperture radar (SAR), electro-optical imagery (EO), hydrophone audio, radio-frequency emissions, and textual maritime reports — into a shared representation where one vessel's observations resolve to a single identity, even after its AIS beacon goes dark. The model trains itself: while a vessel broadcasts AIS, that identity labels the concurrent radar, optical, acoustic, RF, and text observations, so the model learns the cross-modal association and can re-identify the vessel once it goes dark. Every result carries a calibrated confidence and an evidence trail to the raw inputs, with a human analyst in the loop.
At project start the supporting platform is working open-source code: a real-time, provenance-tracked data substrate, an entity graph (a production successor to an 18-month prototype), an analyst console, and four per-modality detectors (AIS, SAR, EO, acoustic). The new research for Component 1a is the learned fusion model itself, its uncertainty calibration, and the text and RF encoders.
End-state: a synthetic Salish Sea scenario — a vessel disables AIS during a Sentinel-1 satellite pass, is re-detected by SAR, EO and hydrophone, and is re-identified by the model — fully replayable and live at planetar.ca, with public-benchmark evaluation. Solo Zax Analytics execution; public data only; no government-furnished property, no field deployments, no classified content. 
--- END PASTE ---

## Notes

- Do NOT use the optional file upload (Field 05).
- If DIP rejects as over 2,000: trim the last sentence (Canadian-IP grounding…) first, then the compute line.
