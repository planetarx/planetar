# Glossary

For the W4 external red-team reader, future audit-prep, and anyone joining the project later. Terms grouped by category. Where a term has a specific meaning *in this bid*, that meaning is given in **bold**; where it's a standard external term, the standard meaning is given.

---

## CH13 / IDEaS / DND program terms

| Term | Meaning |
|---|---|
| **CH13** | Challenge 13 of CFP6: *"Multi-modal AI for advanced situational decisions"*. Solicitation number W7714-248676/013. |
| **CFP6** | Call for Proposals 006. Umbrella for multiple challenges, rolling close 2027-03-31; CH13's specific deadline is 2026-06-02 14:00 EDT. |
| **IDEaS** | Innovation for Defence Excellence and Security — DND/CAF's R&D funding program. |
| **DND/CAF** | Department of National Defence / Canadian Armed Forces. |
| **DIP** | Defence Innovation Portal — the only accepted submission channel (`https://defence-innovation-portal.my.site.com/`). |
| **PWGSC** | Public Works and Government Services Canada (now Public Services and Procurement Canada / PSPC). The contracting authority. |
| **Component 1a** | The TRL 1–3, ≤$250K, ≤6-month, "Conceive" funding bracket of IDEaS Competitive Projects. **planetar's target.** |
| **Component 1b / 1c** | Subsequent Competitive-Projects brackets (1b: TRL 4–5 design phase, ≤$1.5M / 12 mo; 1c-equivalent: TRL 6–9 build phase, ≤$5M). Out of scope for this bid; referenced as follow-on path. |
| **TRL** | Technology Readiness Level (1–9 scale). 1a accepts TRL 1–3. **The bid claims TRL 3 at start.** |
| **GFP** | Government Furnished Property — DND data, systems, personnel, or equipment. **NOT available at 1a.** |
| **MC** | Mandatory Criteria — pass/fail gates. MC-1 (TRL + R&D done), MC-2 (alignment with S&T challenge). |
| **PRC** | Point-Rated Criteria — scored sections. PRC-1 through PRC-7. ≥70/100 required to be responsive. |
| **SC** | Screening Criteria — pre-mandatory checks. SC-1 (financial: ≤$250K, first-half ≤70 % of total). |
| **GBA Plus** | Gender-Based Analysis Plus — Canadian framework for assessing how diverse populations experience policies / technologies. PRC-5 evaluates application to the **technical solution**, not the company. |
| **ISR** | Intelligence, Surveillance, Recognition. The CH13 mission domain. |
| **C2** | Command and Control. Adjacent CH13 mission domain. |
| **CAF Digital Campaign Plan** | DND's digital-modernisation programme that CH13 supports. |

---

## Maritime domain

| Term | Meaning |
|---|---|
| **AIS** | Automatic Identification System — a vessel's transponder broadcast (position, heading, speed, MMSI, name). ITU-R M.1371. **Disabling AIS** = the "dark vessel" problem. |
| **AIS-off / dark vessel** | A vessel that has switched off its AIS transponder, removing itself from the default maritime picture. **planetar's flagship application target.** |
| **MMSI** | Maritime Mobile Service Identity — 9-digit AIS vessel identifier. |
| **IMO number** | International Maritime Organization vessel ID — distinct from MMSI; persistent across ownership changes. |
| **SAR** | Synthetic Aperture Radar — satellite radar imagery. **Sentinel-1** (ESA Copernicus) is the workhorse. All-weather, day/night. |
| **EO** | Electro-Optical — visible-light imagery (cameras). **Surface EO** = camera footage from on-water platforms (USVs, coastal cameras). |
| **Hydrophone** | Underwater acoustic sensor. **NEPTUNE Canada / ONC** operates Canada's national hydrophone infrastructure. |
| **CFAR** | Constant False Alarm Rate — classical SAR ship-detection threshold method. The 1a's `sar.chip` detector uses CFAR + a CNN chip classifier (xView3 reference baseline). |
| **xView3** | DIU (US DoD) public competition + dataset on dark-vessel detection from Sentinel-1 SAR + AIS, 2021. **The validation reference.** |
| **MarineCadastre** | NOAA public AIS archive for US coastal waters, multi-year history, free bulk download. |
| **GFW** | Global Fishing Watch — public AIS feeds with ML-classified vessel behaviours and dark-event annotations. |
| **ONC** | Ocean Networks Canada — operates cabled hydrophone observatories off the BC coast. **Operationally branded as NEPTUNE Canada at the time of Sattar et al. 2011, the bid's institutional anchor.** |
| **NEPTUNE Canada** | Earlier operational name for ONC's cabled observatory programme. Same institution. |
| **Salish Sea** | The body of water between Vancouver Island, the BC mainland, and Washington State. Covered by ONC hydrophones and Sentinel-1 SAR. **planetar demo geography.** |
| **Maritime Task Group Operations** | One of CH13's named application examples — sonar + RF + visual feeds for anomaly detection with uncertainty + explainable outputs. **planetar's primary CH13 alignment.** |
| **OOR** | Open Ocean Robotics — applicant's spouse's employer. Explicitly **NOT** on the bid; no data, code, or letter of support from OOR. Disclosed as conflict-of-interest exclusion in v1. |

---

## Architecture / systems terms

| Term | Meaning |
|---|---|
| **planetar** | The bid's product name. Slack × Palantir on a nanosecond message bus. |
| **planetar-bus** | Repackaged `zbroker0`. The C-language message broker. |
| **planetar-ui** | Repackaged `sales4` comms-app. The React 19 multi-viewer analyst shell. |
| **planetar-graph** | Repackaged `doibio`. The entity-graph + identity-resolution layer. |
| **zbroker0** | Applicant's existing C broker repo (`~/github/sness23/zbroker0`). Becomes `planetar-bus`. |
| **zmesg** | Applicant's binary envelope wire format (`~/github/sness23/zmesg`). UUIDv7 + ns timestamps + topic + correlation/causation IDs. |
| **doibio** | Applicant's working POC of the patented entity architecture (`~/data/dev/doibio`). ~20 k LOC TypeScript, 35 schemas, 630-line identity-resolution engine, 18-month iteration. |
| **sales4** | Applicant's existing Slack/Discord-style multi-client chat with µs-precision latency instrumentation (`~/github/sness23/sales4`). |
| **crank3** | Applicant's evidence-aggregation pattern repo. Source of the log-score decomposition (prior + per-modality evidence + uncertainty + penalty) used in cross-modal fusion. |
| **LMAX Disruptor** | A high-performance ring-buffer messaging architecture, originally designed at the LMAX exchange (Thompson, Barker, Gee 2011). The lineage `zbroker0` extends. |
| **Aeron** | Open-source efficient reliable transport library descended from the LMAX lineage. Studied as a reference. |
| **CAS** | Compare-And-Swap — atomic CPU instruction. Used for lock-free reserves on the SHM ring. |
| **WAL** | Write-Ahead Log. Append-only durable record of every message, with CRC32 per entry, 64 MB segments, recovery on restart. |
| **SHM** | Shared Memory. The fastest of `zbroker0`'s three transports. |
| **memfd** | Linux memory file descriptor (`memfd_create`) — anonymous shared-memory region with no filesystem path. The SHM ring is one. |
| **eventfd** | Linux event file descriptor — used for wake-up signaling between busy-polling consumers and producers. |
| **SCM_RIGHTS** | Unix-socket control message that hands a file descriptor between processes. `zbroker0`'s control plane uses it to give producers and consumers the memfd + eventfd. |
| **UUIDv7** | Time-ordered UUID format (IETF RFC 9562) — 48-bit timestamp + 80 bits randomness. Sortable by creation time. The envelope ID format. |
| **ULID** | Time-ordered 26-character ID with type prefix (`pty_…`, `ves_…`). doibio's identifier scheme. |
| **protobuf** | Google's Protocol Buffers — binary serialisation format. Used for the envelope's opaque payload by convention (broker doesn't parse it). |
| **Event sourcing** | Architectural pattern where an append-only event log is the source of truth, and queryable state is a *projection* of that log. doibio implements this; planetar inherits. |
| **Log compaction** | Kafka technique for retaining the latest message per key while discarding obsolete history. Referenced as architectural lineage; not implemented in 1a scope. |

---

## ML / methodology terms

| Term | Meaning |
|---|---|
| **JEPA** | Joint-Embedding Predictive Architecture — a self-supervised representation-learning family. Mentioned in v1 (MAIA-MD) framing; **planetar does not commit to a JEPA implementation**, only references the idea as one possible fusion-head choice. |
| **Semi-supervised** | ML training using a small labeled set + a large unlabeled set. Applicant's published methodology (ORCA-SLANG, Sattar et al.) for archive-scale acoustic ML. |
| **Conformal prediction** | Distribution-free uncertainty quantification — produces prediction sets with valid coverage guarantees regardless of the underlying model. Used to calibrate cross-modal fusion scores. (Vovk-Gammerman-Shafer 2005; Angelopoulos-Bates 2021.) |
| **Log-score decomposition** | Scoring scheme: total = prior + Σ per-modality evidence + uncertainty − penalty. Used for cross-modal vessel re-ID. Pattern from applicant's `crank3` codebase. |
| **Levenshtein distance** | Edit distance between two strings. Used in `doibio`'s identity-resolution engine for fuzzy-name matching. |
| **CARFAC** | Cascade of Asymmetric Resonators with Fast-Acting Compression — Richard Lyon's biologically-inspired cochlear model at Google. Background context for the Walters/Lyon Auditory Sparse Coding citation [A3]. |
| **SOM** | Self-Organizing Map (Kohonen map) — unsupervised topology-preserving dimensionality reduction. Applicant's PhD-era publications (PETRA / ICMC 2009). |
| **Marsyas** | Music Analysis, Retrieval and Synthesis — open-source audio-feature-extraction framework, originally by Tzanetakis at UVic. Integrated into the openmir / Orchive system. |
| **The Orchive** | Applicant's PhD project — semi-automatic-annotation system for the OrcaLab hydrophone archive (Hanson Island, BC). Decades of continuous recordings. |
| **ORCA-SLANG** | Bergler et al. 2021 (Interspeech) — multi-stage semi-supervised deep learning pipeline for killer-whale-call type identification. **The methodological template** for the `acoustic.event` detector in planetar. |
| **xView3** | DIU dataset + competition (above, in maritime domain). Provides labeled SAR + AIS pairs for dark-vessel detection. |
| **ShipsEar** | Underwater vessel-noise dataset by class (Santos-Domínguez et al. 2016). Vessel-discrimination training set for `acoustic.event`. |
| **DeepShip** | 47-hour underwater ship-noise benchmark (Irfan et al. 2021). Companion to ShipsEar. |

---

## Latency / measurement terms

| Term | Meaning |
|---|---|
| **p50 / p90 / p99** | 50th / 90th / 99th percentile of a measurement distribution. p50 = median; p99 = the value 99 % of measurements are below. |
| **Hot cache / cold cache** | Whether the CPU cache lines being read are recently touched (hot) or evicted (cold). p50 latency is sensitive; p99 less so. |
| **isolcpus** | Linux kernel boot parameter that excludes CPU cores from the scheduler. Used in latency-tuned deployments; **NOT** used in planetar's reported numbers. |
| **SCHED_FIFO** | Linux real-time scheduler policy. **NOT** used in planetar's reported numbers — production-grade tuning kept out of the 1a claim. |
| **busy-poll** | Loop that repeatedly checks a memory location until it changes, instead of blocking on a kernel wait. Lowest latency; high CPU. `consumer-ultra` does this with an eventfd fallback. |
| **kernel bypass** | Technique that skips the kernel's network stack (DPDK, RDMA, etc.) for sub-microsecond performance. **NOT** used in planetar's reported numbers. |

---

## Project-internal acronyms

| Term | Meaning |
|---|---|
| **MAIA-MD** | The v1 (zdefence/) bid working name — *Multi-modal AI for Maritime Detection*. Single-model JEPA fusion. **Superseded by planetar; kept for reference.** |
| **v1 / v2** | v1 = the MAIA-MD bid in `~/github/sness23/zdefence/`. v2 = the planetar bid in `~/github/planetarx/planetar/` (this directory). The v2 is the intended submission. |
| **W0–W5** | Proposal-calendar weeks (per `07-TIMELINE.md`). W0 = directory creation; W5 = final freeze + submit. |
| **M1–M6** | 6-month execution-calendar milestones (post-award). M1 = bus hardening; M6 = demo + final report. |

---

## Cross-references

- Reference list with full citation IDs `[A1] … [F5]`: `06-REFERENCES.md`.
- Architecture in prose: `03-ARCHITECTURE.md`.
- Architecture in diagrams: `docs/architecture-diagrams.md`.
- Open questions / decision audit trail: `08-OPEN-QUESTIONS.md`.
