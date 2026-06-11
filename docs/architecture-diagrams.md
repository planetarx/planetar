# Architecture Diagrams

Mermaid source for diagrams referenced in the bid. Renders inline on GitHub, GitLab, Obsidian, VS Code, and most modern markdown viewers.

The ASCII version of (1) lives in `03-ARCHITECTURE.md` for portability. These Mermaid versions are for reviewer-facing decks, the proposal cover-page (if a graphic is requested at contract negotiation), and the 1b proposal package.

---

## (1) System overview — five layers

The five-layer composition is the architectural thesis. Every component in the system is on the bus; nothing talks directly.

```mermaid
flowchart TB
    subgraph Ingress[" Layer 0 · Ingress adapters "]
        AIS[AIS<br/>MarineCadastre / GFW]
        SAR[Sentinel-1 SAR<br/>Copernicus]
        EO[Surface EO<br/>Singapore Maritime / MODS]
        HYD[Hydrophone<br/>ONC / ShipsEar / DeepShip]
        RF[Non-AIS RF<br/>stub topic]
    end

    subgraph Envelope[" Layer 1 · zmesg envelope "]
        ENV["UUIDv7 · ns timestamps · topic<br/>correlation_id · causation_id<br/>opaque payload (protobuf)"]
    end

    subgraph Bus[" Layer 2 · planetar-broker  ·  SHM p50 = 80–140 ns / p99 = 400–900 ns (predecessor zbroker0, 2026-04-27) "]
        SHM[SHM ring<br/>16 MB memfd<br/>lock-free CAS]
        TCP[TCP bridge<br/>:12001 / :12002]
        UDP[UDP bridge<br/>:12003]
        WAL[(WAL<br/>CRC32 segments<br/>replay-from-cursor)]
    end

    subgraph Detectors[" Layer 3 · Detectors "]
        AISGAP["ais.gap<br/>(heuristic)"]
        SARCHIP["sar.chip<br/>(CFAR + CNN, xView3)"]
        EOCHIP["eo.chip<br/>(fine-tuned detector)"]
        ACOUSTIC["acoustic.event<br/>(ORCA-SLANG semi-supervised)"]
        REID["vessel.ReIDCandidate<br/>(cross-modal fusion · 1a research)"]
    end

    subgraph Graph[" Layer 4 · Entity graph (US Patent 10,936,582 + doibio POC) "]
        VESSEL[Vessel entity]
        VESSELID[VesselIdentification<br/>MMSI · IMO · acoustic · RF · OCR]
        SOURCE[ObservationSource<br/>provenance + confidence]
        VESSEL --- VESSELID
        VESSEL --- SOURCE
    end

    subgraph Shell[" Layer 5 · planetar-ui shell (sales4 base) "]
        MAP[Map viewer]
        TIMELINE[Timeline viewer]
        ENTITY[Entity-card viewer]
        WAVE[Waveform viewer]
        CHANNEL[Channel viewer]
    end

    Ingress --> ENV
    ENV --> SHM
    SHM <--> TCP
    SHM <--> UDP
    SHM --> WAL
    SHM --> Detectors
    SHM --> Graph
    SHM --> Shell
    Graph -.-> SHM
```

The dotted line from Graph back to SHM is the **write-back**: when the entity graph publishes `vessel.v1.ReIDCandidate`, that publication is itself a bus message — the shell renders it, the WAL records it, and reviewers can reconstruct graph state at any historical `t`.

---

## (2) Dark-vessel scenario — concrete data flow

The flagship 1a demonstration, expressed as a sequence of typed bus messages with `causation_id` lineage.

```mermaid
sequenceDiagram
    autonumber
    participant Ingest as Ingress
    participant Bus as planetar-broker bus
    participant AISGap as ais.gap
    participant SAR as sar.chip
    participant Acoustic as acoustic.event
    participant Graph as Entity graph
    participant UI as planetar-ui

    Ingest->>Bus: ais.v1.Position {mmsi, t, lat, lon}
    Ingest->>Bus: ais.v1.Position {mmsi, t+30s, ...}
    Note over AISGap: monitors expected interval
    Bus->>AISGap: ais.v1.Position stream
    Note over Ingest: vessel disables AIS
    Ingest-->>Bus: (no further AIS)
    AISGap->>Bus: vessel.v1.WentDark<br/>{mmsi, last_known_pos, t_dark}
    Bus->>UI: WentDark surfaced in map + timeline

    Ingest->>Bus: sar.v1.Tile (Sentinel-1 pass)
    Bus->>SAR: tile
    SAR->>Bus: sar.v1.Detection<br/>{lat, lon, chip, confidence}<br/>causation_id ← tile

    Ingest->>Bus: acoustic.v1.Frame (hydrophone)
    Bus->>Acoustic: frame
    Acoustic->>Bus: acoustic.v1.Event<br/>{vessel-class, t, station_id}<br/>causation_id ← frame

    Bus->>Graph: WentDark + Detection + Event
    Note over Graph: spatiotemporal correlation<br/>+ identity-resolution match
    Graph->>Bus: vessel.v1.ReIDCandidate<br/>{entity_id, evidence: [msg_id...], score}<br/>causation_id ← all three inputs
    Bus->>UI: ReIDCandidate surfaced in entity-card<br/>analyst clicks through to inputs
```

Step (12) — `vessel.v1.ReIDCandidate` — is the system's output. The analyst sees one candidate entity with a list of evidence messages; clicking any of them traces back through `causation_id` to the raw input envelope. **Native lineage, not post-hoc XAI.**

---

## (3) Bus internals — control plane vs data plane

```mermaid
flowchart LR
    subgraph Control[" Control plane "]
        SOCK["/tmp/broker-ultra.sock<br/>SOCK_SEQPACKET"]
        HANDOFF["SCM_RIGHTS<br/>memfd + eventfd handoff"]
        ADMIN[":7001 admin HTTP<br/>(ring depth, subscriber count)"]
    end

    subgraph DataPlane[" Data plane (no syscalls on hot path) "]
        RING["SHM ring<br/>16 MB memfd<br/>cache-aligned write_idx"]
        CAS["lock-free CAS reserves<br/>fetch_add → write entry → atomic_store commit"]
        BUSY[Busy-poll consumer<br/>+ eventfd wake-up fallback]
    end

    subgraph Durability[" Durability "]
        WAL[(WAL segments<br/>64 MB mmap'd<br/>CRC32 per entry<br/>auto-rotation)]
        OFFSETS["offsets.dat<br/>per-consumer cursors"]
        REPLAY["./writer-wal replay<br/>./writer-wal stream"]
    end

    Producer -->|connect| SOCK
    Consumer -->|connect| SOCK
    SOCK --> HANDOFF
    HANDOFF --> RING
    RING --> CAS
    CAS --> BUSY
    RING --> WAL
    WAL --> OFFSETS
    WAL --> REPLAY
```

**Control-plane and data-plane separation is what delivers nanosecond latency on the data path.** Connection / subscription / fd-handoff happens once via a Unix socket; thereafter, everything is direct memory access.

---

## (4) Comparison — planetar vs. conventional ISR fusion stack

```mermaid
flowchart LR
    subgraph Conventional[" Conventional ISR fusion stack "]
        C1[Per-modality pipeline 1] --> CB[(Central<br/>fusion DB)]
        C2[Per-modality pipeline 2] --> CB
        C3[Per-modality pipeline 3] --> CB
        CB --> CO[Operator console]
        CB --> CX[XAI module<br/>post-hoc]
        Note1["ms-class internal latency<br/>per-modality silos<br/>XAI bolted-on"]
    end

    subgraph Planetar[" planetar "]
        P1[Ingress 1] -.envelopes.-> PB([nanosecond bus])
        P2[Ingress 2] -.envelopes.-> PB
        P3[Ingress 3] -.envelopes.-> PB
        P4[Ingress 4] -.envelopes.-> PB
        PB --> PD[Detectors]
        PD --> PB
        PB --> PG[Entity graph]
        PG --> PB
        PB --> PUI[planetar-ui<br/>map · timeline · entity-card · waveform · channel]
        Note2["80–140 ns p50<br/>everything is on the bus<br/>causation_id native lineage"]
    end
```

The **same architectural decision** — every component is a bus participant, nothing else — eliminates the silos, the fusion-DB bottleneck, and the bolted-on XAI in one move.

---

## Rendering notes

- All four diagrams render natively on **GitHub** without configuration.
- For the proposal package (if a graphic is requested at contract negotiation), export to SVG via Mermaid CLI: `mmdc -i architecture-diagrams.md -o diagrams.svg`.
- Diagram (1) has an ASCII-art twin in `03-ARCHITECTURE.md` for portability when SVG / Mermaid isn't available.

## Cross-references

- Layer-by-layer prose: `03-ARCHITECTURE.md`.
- Bus measurement: `docs/benchmark-2026-04-27.md`.
- Detector definitions and topic schemas: `03-ARCHITECTURE.md` Layer 3.
- doibio entity-graph lineage: `03-ARCHITECTURE.md` Layer 4.
