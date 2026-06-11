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
| **MarineCadastre.gov** | NOAA | US coastal AIS archives, multi-year | Free bulk download |
| **Danish Maritime Authority** | dma.dk | European waters, raw AIS | Free bulk download |
| **AISHub** | aishub.net | Real-time + historical | Free for non-commercial |
| **Global Fishing Watch** | globalfishingwatch.org | AIS + ML-classified vessel behaviours, gear types, dark-event annotations | API key free for research |

The `ais.gap` detector is a heuristic on continuous AIS streams; ground truth for "this gap was a deliberate dark event" comes from xView3 (which co-locates Sentinel-1 with AIS at acquisition time) and Global Fishing Watch's curated dark-event labels.

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

## Non-AIS RF (where public data exists)

This modality is the weakest of the five for public data. Stretch goal, not core deliverable.

| Dataset | Source | Size / scope | Notes |
|---|---|---|---|
| **HawkEye 360 RFGeo (selective)** | hawkeye360.com | Commercial RF-geolocation. Limited public samples. | Some public case studies. Commercial data not in scope unless freely sampled. |
| **Sentinel-1 SAR side-channel** | Copernicus | RFI artifacts in SAR data | Indirect — RFI is a known nuisance signal in S1 GRD that can be repurposed |
| **OpenSky Network** | opensky-network.org | ADS-B (aviation), not maritime RF, but pattern-of-life template | Free academic |

The 1a treats `rf.emit` as an additional bus topic; the detector is a placeholder (not a research deliverable). 1b proposal explores RF more deeply.

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
| M1 | None — bus hardening |
| M2 | Stand up four ingresses: AIS (MarineCadastre + GFW), SAR (Sentinel-1 via Copernicus), EO (Singapore Maritime), Hydrophone (ONC + ShipsEar) |
| M3 | Train / fine-tune detectors on labeled subsets (xView3 for SAR, Singapore Maritime for EO, ShipsEar/DeepShip for acoustic, GFW dark-event labels for `ais.gap`) |
| M4 | Cross-modal evaluation set: build the synthetic Salish-Sea-dark-event scenario from real AIS gap + concurrent Sentinel-1 + ONC hydrophone |
| M5 | Viewer demo on the curated scenario |
| M6 | Public-benchmark evaluation report |

## What is NOT in scope

- **No DND data** (GFP unavailable at 1a).
- **No classified data**.
- **No commercial RF datasets** (HawkEye 360, etc.) unless public samples suffice.
- **No personal data** (the bid is about vessels, not people; AIS MMSI is a vessel identifier, not PII).
- **No frontier-scale pretraining**. All detectors are fine-tunes of public baselines or applicant's own published methodology.
- **No data acquisition fieldwork** (no boats, no sensor purchases, no ONC field deployments — public archives only).
