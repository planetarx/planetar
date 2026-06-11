# 05 — Public Datasets

All training and evaluation data for the 1a is open-source or synthetic. GFP is unavailable at 1a (CFP §3.5.4); classified data not accepted. No ITAR-restricted data, no commercial-only data unless trivially licensed.

## SAR (synthetic aperture radar — satellite imagery)

| Dataset | Source | Size / scope | Notes |
|---|---|---|---|
| **Sentinel-1** | ESA Copernicus Open Access Hub | C-band, GRD + SLC, ~6-day revisit, global | Workhorse. Free programmatic access. |
| **xView3** | DIU (US DoD), 2021 | Sentinel-1 + co-located AIS, dark-vessel-labeled | The validation reference. **Cite prominently in PRC-1(a).** |
| **SAR-Ship-Dataset** | Wang et al. 2019 | 43k+ chips from Sentinel-1 + Gaofen-3 | Free (GitHub) |
| **HRSID** | Wei et al. 2020 | 5,604 high-res chips, 16,951 instances | Free |
| **SSDD** | Li et al. 2017 | 1,160 images, first public SAR ship dataset | Free |
| **FUSAR-Ship** | Hou et al. 2020 | 126k chips from Gaofen-3, 98 cargo classes | Free |

planetar uses Sentinel-1 GRD as the live ingress modality and xView3 as the labeled-evaluation backbone. The 1a `sar.chip` detector is a CFAR + chip-classifier baseline ported from the public xView3 reference implementations, not a frontier model.

## AIS (vessel transponder broadcasts)

| Dataset | Source | Size / scope | Notes |
|---|---|---|---|
| **aisstream.io** | aisstream.io | Real-time WebSocket, global coverage where volunteer receivers exist | **Free, no SLA** (community/volunteer-fed, BETA). Used today by `planetar-ais` for live Victoria, BC ingest. |
| **MarineCadastre.gov** | NOAA | US coastal AIS archives, multi-year | Free bulk download |
| **Danish Maritime Authority** | dma.dk | European waters, raw AIS | Free bulk download |
| **AISHub** | aishub.net | Real-time + historical | Free for non-commercial; requires operating a receiver |
| **Global Fishing Watch** | globalfishingwatch.org | AIS + ML-classified vessel behaviours, gear types, dark-event annotations | API key free for research |

The `ais.gap` detector is a heuristic on continuous AIS streams; ground truth for "this gap was a deliberate dark event" comes from xView3 (which co-locates Sentinel-1 with AIS at acquisition time) and Global Fishing Watch's curated dark-event labels.

**Live AIS — cost during R&D vs. operationalization.** The Component 1a demo ingests live AIS via aisstream.io at $0 — no licensing, no per-message billing, no bandwidth fee. This keeps the prototype's AIS-feed cost line at zero through the 6-month build. An operational deployment would require a paid feed for SLA, licensing, and offshore (satellite-AIS) coverage. Indicative pricing of established alternatives: MarineTraffic API from ~US$700/mo (regional), VesselFinder API from ~US$200/mo (regional), Spire Maritime and Orbcomm (global satellite-AIS) typically 5-figure annual contracts. `planetar-ais` is feed-agnostic — switching sources is a single adapter swap (`src/source-*.mjs`) and no contract change for downstream consumers, so the operational-AIS line is a procurement decision, not an engineering one.

## Maritime EO / surface camera

| Dataset | Source | Size / scope | Notes |
|---|---|---|---|
| **Singapore Maritime Dataset** | Prasad et al. 2017 | On-water mounted-camera footage, day + night, bbox-labeled | Free (registration) |
| **MODS** | Bovcon et al. 2022 | USV-perspective obstacle detection, stereo + IMU | Free |
| **MaSTr1325** | Bovcon & Kristan 2019 | 1,325 semantic-seg images (water/sky/obstacle) | Free |
| **WaSR** | Bovcon & Kristan 2021 | Water seg + obstacle detection net + data | Free |
| **SeaShips** | Shao et al. 2018 | 7k+ vessel images from coastal surveillance | Free |
| **ABOShips** | Iancu et al. 2021 | 9,880 ships with class annotations | Free |

The `eo.chip` detector is a fine-tuned public detector (YOLOv8 / RT-DETR class) on Singapore Maritime + MODS — same SOTA-but-public approach as `sar.chip`.

## Passive acoustic / hydrophone

This modality carries the strongest applicant-credential anchor in the bid (see `04-PORTFOLIO.md`).

| Dataset | Source | Size / scope | Notes |
|---|---|---|---|
| **Ocean Networks Canada (ONC)** | oceannetworks.ca | Real-time + archived hydrophone from cabled observatories off BC | Free (registration). Naxys 02345 @ 96 kHz / 3000 m is the same instrument class as Sattar et al. 2011 |
| **MobySound** | mobysound.org | Marine mammal acoustic archive | Free |
| **DCLDE bioacoustic challenges** | various | Annual detection-classification-localization-density-estimation challenges | Free per challenge |
| **Watkins Marine Mammal Sound Database** | Woods Hole | Curated marine mammal recordings | Free |
| **NOAA Passive Acoustic Archive** | ncei.noaa.gov | US ocean acoustic monitoring | Free |
| **ShipsEar** | Santos-Domínguez et al. 2016 | Underwater vessel noise by class (90 recordings, 11 vessel types) | Free |
| **DeepShip** | Irfan et al. 2021 | 47 hours underwater ship noise | Free |

ShipsEar + DeepShip are the **vessel-discrimination** training sets; ONC + MobySound + DCLDE are the **deployment-realistic** evaluation streams (continuous, noisy, unlabeled-by-default — the regime Sattar et al. 2011 worked in). The `acoustic.event` detector follows the ORCA-SLANG semi-supervised template: large unlabeled archive + small labeled seed → multi-stage self-training → calibrated event classifier.

## Text / maritime reports (NLP)

The modality CH13 names explicitly ("text reports", "text intelligence"). A transformer encoder (BERT-class [I7]) embeds reports into the shared fusion space alongside the sensor modalities; text observations link to vessel entities by name / IMO / callsign mention and resolve through the same entity graph.

| Source | Scope | Notes |
|---|---|---|
| **Notices to Mariners** (NGA, Canadian Coast Guard) | Navigational warnings, restricted-area and vessel notices | Free, public |
| **Port State Control reports** (Paris / Tokyo MoU, Transport Canada) | Per-vessel inspection / detention records | Free, public; per-vessel links |
| **Equasis** | Vessel particulars, ownership, history (text fields) | Free (registration) |
| **OSINT / maritime advisories** | Sanctions lists, IMB piracy / incident reports | Public; deduplicated, source-tagged |

No classified or restricted SIGINT — public text only at 1a.

## Non-AIS RF (where public data exists)

RF is the hardest modality to source publicly. The 1a builds a **real** RF-signature encoder and evaluates it on best-effort public and synthetic data, with a documented fallback (PRC-4) to a five-modality model if data proves insufficient — no overclaim.

| Dataset | Source | Size / scope | Notes |
|---|---|---|---|
| **HawkEye 360 RFGeo (selective)** | hawkeye360.com | Commercial RF-geolocation. Limited public samples. | Some public case studies. Commercial data not in scope unless freely sampled. |
| **Sentinel-1 SAR side-channel** | Copernicus | RFI artifacts in SAR data | Indirect — RFI is a known nuisance signal in S1 GRD that can be repurposed |
| **OpenSky Network** | opensky-network.org | ADS-B (aviation), not maritime RF, but pattern-of-life template | Free academic |

The 1a builds a real `rf.emit` encoder into the fusion model; where public / synthetic RF is thin, the five-modality model (AIS / SAR / EO / acoustic / text) is the no-overclaim fallback. 1b explores RF more deeply.

## Synthetic / simulated fallbacks

For controlled evaluation of dark-vessel scenarios where labeled real data is sparse:

| Tool | Generates |
|---|---|
| **Procedural AIS dropout** | Take MarineCadastre / GFW tracks, drop segments, generate synthetic dark events with ground truth |
| **Sentinel-1 backscatter simulation** | Match SAR chip statistics to known vessel classes for adversarial coverage |
| **Gazebo + UUV Simulator** | Maritime sensor sim (radar, sonar, camera) |
| **Unity Perception** | Synthetic imagery with pixel labels, domain randomization |
| **Unreal + AirSim** | High-fidelity sensor sim |

Synthetic data is **never** the only training data for any modality. It's used for adversarial evaluation and edge-case coverage.

## Data strategy aligned with the 6-month timeline

Mapping to `07-TIMELINE.md` execution calendar (M1–M6):

| Month | Data work |
|---|---|
| M1 | Substrate + ingest hardening; stand up text + RF ingest scaffolding |
| M2 | Per-modality encoders; build the **AIS-on co-occurrence training set** (AIS identity labels concurrent SAR / EO / acoustic / RF / text) |
| M3 | Train the self-supervised cross-modal fusion model; xView3 [D1] as supervised anchor |
| M4 | Conformal calibration; AIS-off dark-vessel evaluation set — synthetic Salish-Sea scenario from real AIS gap + concurrent Sentinel-1 + ONC hydrophone |
| M5 | Explainable + human-in-the-loop surface on the curated scenario |
| M6 | Public-benchmark evaluation report |

## What is NOT in scope

- **No DND data** (GFP unavailable at 1a).
- **No classified data**.
- **No commercial RF datasets** (HawkEye 360, etc.) unless public samples suffice.
- **No personal data** (the bid is about vessels, not people; AIS MMSI is a vessel identifier, not PII).
- **No frontier-scale pretraining**. All detectors are fine-tunes of public baselines or applicant's own published methodology.
- **No data acquisition fieldwork** (no boats, no sensor purchases, no ONC field deployments — public archives only).
