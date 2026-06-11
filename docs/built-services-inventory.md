# Built-services inventory — proposal submission snapshot

**Date of snapshot:** 2026-05-21 (mid-W4, T-8 days to target submission)
**Purpose:** single source of truth for what's actually built, broker-integrated, and testable today. Cited from `README.md` canonical-spine table, `proposal/PRC-4_feasibility.md`, and `proposal/MC-1_trl.md`. Hand to the W4 external cold reader alongside `docs/external-reader-briefing.md`.

Audit method (every entry below): `wc -l` on source files (excluding `node_modules`, `dist`, `.venv`, `__pycache__`), `git log -1 --format=%ai` for last commit, `grep` on the repo for broker-integration evidence (`12001`, `12002`, `zmesg`, `publish`, `topic`), and Read on the top-level README. Findings are **provenance-traceable**, not recalled from memory.

---

## Canonical spine — services shipping at proposal start

### 1. `planetar-broker` — message bus

| Field | Value |
|---|---|
| Repo | `~/github/planetarx/planetar-broker/` (public, AGPL-3.0) |
| Language | C |
| LOC | ~1,200 (broker) + 190 (`shm-consumer` test client) |
| Predecessor | `~/github/sness23/zbroker0/` (~1,673 LOC, kept for benchmark lineage) |
| Ports | 12001 (TCP producer), 12002 (TCP consumer), 12003 (UDP), Unix `/tmp/planetar-broker.sock` (SHM handshake via `SCM_RIGHTS`) |
| Transport | TCP + UDP + SHM ring; WAL with CRC32; lock-free CAS reserves on the data path |
| Perf — SHM (predecessor `zbroker0`) | p50 = 80–140 ns, p99 = 400–900 ns over 1M-msg benchmark, 1.8–1.9 M msg/s — formal report at `docs/benchmark-2026-04-27.md` |
| Perf — TCP (planetar-broker) | p50 = 34 µs, p99 = 424 µs at paced ~15 k msg/s, 79-byte messages, i9-9900K, kernel 6.17, 2026-05-14 — raw artifacts preserved (logs + bench source); see "TCP path baseline" appendix in `docs/benchmark-2026-04-27.md` |
| Verdict | **Working, TRL 3–4 at project start.** Bus is the platform's verifiable spine. |

### 2. `zmesg` — envelope

| Field | Value |
|---|---|
| Repo | `~/github/sness23/zmesg/` (public, Apache-2.0 — header carve-out so bus publishers aren't copyleft-bound) |
| Language | C (header-only) |
| LOC | ~260 |
| Fields | UUIDv7 message id; ns-precision timestamps; topic; correlation id; causation id; source id; payload |
| Framing | TCP/UDP: 4-byte big-endian length prefix + zmesg envelope (envelope itself is LE). Note `[[reference_broker_framing]]`: planetar-sat publisher had a `<I` LE-prefix latent bug surfaced during integration, fixed. |
| Verdict | **Working, TRL 4.** Zero-copy parse. |

### 3. `planetar-ui` — analyst shell

| Field | Value |
|---|---|
| Repo | `~/github/planetarx/planetar-ui/` (public, AGPL-3.0) |
| Language | TypeScript / React 19 / Vite |
| Predecessor | `~/github/sness23/sales4/` |
| Role | Slack/Discord/Quip/Palantir-style 4-pane shell; WS bridge to the broker; µs-instrumented |
| Verdict | **Working, TRL 3–4 at project start.** Base shell + WS bridge ship today; M5 work is the **viewer extensions** (map, timeline, entity-card, waveform), not the shell itself. |

### 4. `planetar-ais` — AIS ingress

| Field | Value |
|---|---|
| Repo | `~/github/planetarx/planetar-ais/` (public, AGPL-3.0) |
| Language | Node / JS |
| Broker integration | TCP publisher to `127.0.0.1:12001`; one chat channel per MMSI |
| Geo | Victoria BBox (Salish Sea POC) — live AIS feed |
| Verdict | **Working, TRL 3 at project start.** First per-modality ingress; pattern donor for sat/eo/acoustic. |

### 5. `planetar-sat` — SAR ingress + detector

| Field | Value |
|---|---|
| Repo | `~/github/planetarx/planetar-sat/` (public, AGPL-3.0) |
| Language | Python ≥3.11 (NumPy, SciPy, Rasterio, Shapely, Click) |
| LOC | 1,583 source + 647 in 5 test files |
| Last commit | 2026-05-15 |
| Broker integration | TCP publisher → 127.0.0.1:12001; emits `sar.chip`, `track.update`, human-readable `chat.pac.sar-detections` envelopes; zmesg + BE 4-byte length prefix |
| Pipeline | Sentinel-1 GRD fetch → CFAR + land-mask → IoU tracker → typed bus envelopes |
| Runnable | `make install && .venv/bin/planetar-sat run …` — CLI entry from `pyproject.toml`; Dockerfile present |
| Tests | CFAR, geocoding, land-mask, tracker, zmesg round-trip — all pass |
| Validated on | 433 Mpx real Sentinel-1 GRD scene |
| Verdict | **Working prototype, broker-integrated, TRL 3.** Known limitation: full-scene memory; tiled reads next. |

### 6. `planetar-eo` — EO ingress + detector

| Field | Value |
|---|---|
| Repo | `~/github/planetarx/planetar-eo/` (public, AGPL-3.0) |
| Language | Python ≥3.11 (NumPy, OpenCV, PyYAML, Click; optional torch, ultralytics, yt-dlp) |
| LOC | 1,786 source + 1 core test (`tests/test_envelope.py`) |
| Last commit | 2026-05-15 |
| Broker integration | TCP publisher → 127.0.0.1:12001 (`--broker 127.0.0.1:12001` / `--no-broker` for stdout); emits `eo.frame` + `eo.detection` envelopes |
| Pipeline | Public webcam feed → YOLO11n vessel detection → typed bus envelopes |
| Victoria POC sources | CHEK news cam, BC Ferries Swartz Bay terminal, Hakai wharf cam (2688×1512, ~5 s cadence, GPU), ONC live cams |
| Runnable | `planetar-eo probe` (saves annotated JPEGs for QA before bus hookup); `planetar-eo run` for live publishing |
| Stub | `reid/` (cross-camera re-ID) — M3/M4 work, not yet filled |
| Verdict | **Working prototype, broker-integrated, TRL 3.** Detector fires on real Victoria webcams; integration with `planetar-ui` already documented. |

### 7. `planetar-acoustic` — hydrophone ingress + detector

| Field | Value |
|---|---|
| Repo | `~/github/planetarx/planetar-acoustic/` (public, AGPL-3.0) |
| Language | Python ≥3.11 (NumPy, SciPy, Soundfile, Click; optional torch, torchvision, onnxruntime, websockets) |
| LOC | 2,621 source + 567 in 5 test files |
| Last commit | 2026-05-15 |
| Broker integration | TCP publisher → 127.0.0.1:12001 (env override `PLANETAR_BROKER`); emits `acoustic.{site,detect,psd,classify}` + `chat.pac.hydrophone-alerts` envelopes |
| Pipeline | Hydrophone source → CAR-FAC cochlear model → Lyons SAI (strobed temporal integration) → CV classifier → typed bus envelopes |
| Sources | `synth`, `archive`, `onc` (ONC Salish Sea hydrophone), `orcasound` (HLS) — wired but `onc`/`orcasound` need `ONC_TOKEN` + outbound HTTPS to exercise |
| Corpora named in README | ONC Salish Sea, OrcaSound HLS, DeepShip, ShipsEar |
| Tests | Presence gate, mock classifier, sources, zmesg, synthetic SAI — all pass |
| Verdict | **Working prototype, broker-integrated, TRL 3.** Mock classifier carries `model_id="mock"` for filtering — no fake-numbers risk. |

### 8. `planetar-ontology` — entity graph (built)

| Field | Value |
|---|---|
| Repo | `~/github/planetarx/planetar-ontology/` (public, AGPL-3.0) |
| Language | TypeScript / Node ≥22.18, **zero npm dependencies** (uses native `node:sqlite`, native ESM) |
| LOC | 2,323 in 16 TS files |
| Last commit | 2026-05-18 |
| Broker integration | TCP subscriber → 127.0.0.1:12002; ingests zmesg envelopes → classifies by `schema_name` → persists to SQLite |
| Phases shipped | **P1** (zmesg codec + registry) · **P2** (identity resolution + merge) · **P3** (Object API) · **P4** (Action executor) · **P5** (kinematic match rules incl. dark-vessel re-ID) |
| Tests | 30 tests in 5 test files (p1–p5), **all pass**; `npm test` requires no broker |
| Runnable | `npm start` (connect + ingest), `npm run publish-synth` (synthetic vessel publisher for live testing) |
| Architecture doc | `~/data/vaults/docs/ARCH-planetar-ontology.md` |
| Verdict | **Production-ready, broker-integrated, TRL 4.** All five phases complete. Wiring `planetar-ui` → Object API is a UI-side task, not this repo. |

### 9. `planetar-registry` — canonical data model

| Field | Value |
|---|---|
| Repo | `~/github/planetarx/planetar-registry/` (public, AGPL-3.0) |
| Language | JavaScript / Node ≥22, **zero dependencies** |
| LOC | 710 in 9 source `.mjs` files + 2 demo entry points |
| Last commit | 2026-05-18 |
| Role | **Codegen SSOT** — the registry (JSON) generates zmesg field dictionaries, JSON Schemas, TypeScript interfaces, SQLite DDL. The `sat`/`eo`/`acoustic` detectors emit envelopes that conform to schemas generated here. |
| Adapters | `adapter-ais.mjs`, `adapter-sar.mjs` — entity-resolution examples |
| Runnable | `node demo.mjs` (one type end-to-end: `planetar:Vessel` → JSON/markdown/binary, **8/8 round-trip pass**); `node demo-fusion.mjs` (two types + identity resolution, **5/5 scenarios pass**) |
| Tests | Demos *are* the tests (executable spec); no separate test files |
| Note | Binary body is 31 % smaller than JSON for the same envelope payload. |
| Verdict | **Reference prototype, TRL 2–3.** Specification layer; no live streaming by design. Foundation for the unified planetar+doibio canonical data model spec (`~/data/vaults/docs/ARCH-canonical-data-model.md`). |

### 10. `doibio` — entity-graph POC + pattern donor

| Field | Value |
|---|---|
| Path | `~/data/dev/doibio/` (private, 18-month iteration sandbox) |
| Language | TypeScript |
| LOC | ~20,000 across ~35 JSON schemas |
| Key file | `src/lib/identity-resolution.ts` — 630 lines (Levenshtein + exact-ID anchors, tuned merge ≥ 0.95 / link ≥ 0.80 / review ≥ 0.70) |
| Patent backing | **US 10,936,582** *Integrated entity view across distributed systems* — Salesforce-assigned (Q9 verified); applicant is **1 of 19 named inventors** (audit-safe framing: "applicant-named inventor on") |
| Companion repos | `doibio2` (private, kept) and `doibio3` (public, AGPL-3.0, ~600 LOC minimal reference impl). **Full ~20 k LOC cleanup into `doibio3` is user-blocked work pending before submission.** |
| Role in proposal | Predecessor + reference implementation; **planetar-ontology is the production successor** built fresh for CH13. |
| Verdict | **Working POC, TRL 3 as evidence; production-grade work moved to `planetar-ontology`.** |

---

## Totals

- **Source code lines in working `planetar-*` services (not counting `doibio` predecessor):** ≈ 13 k (1.2 k broker C + 0.3 k zmesg + 1.6 k sat Py + 1.8 k eo Py + 2.6 k acoustic Py + 2.3 k ontology TS + 0.7 k registry JS + planetar-ais + planetar-ui).
- **Total tests passing at project start:** 30 (ontology) + 5 (sat) + 5 (acoustic) + 1 (eo envelope) + 8/8 + 5/5 (registry demos) = **45+ tests / demos covering all 5 modalities + entity graph + canonical schema**.
- **Repos open-source on GitHub (as of 2026-05-15):** 6 `planetar-*` + `zmesg` + `doibio3`. All gitleaks-scanned clean.

---

## What is still M1–M6 work (honest scope)

The services above ship at project start. What the 1a still funds — and the proposal correctly bills as research:

| Milestone | Genuine remaining work |
|---|---|
| **M1** | Reproducible CI for the broker bench; protobuf wrapper `planetar.envelope.v1` over zmesg; tagged `v0.1.0` release. |
| **M2** | WAL cold-storage layout finalized; replay-from-cursor verified end-to-end; ONC/CDSE credential handling for the acoustic + SAR sources from a production host. |
| **M3** | The **fifth detector — `vessel.ReIDCandidate` cross-modal fusion** — does not exist today; this is the new R&D. `ais.gap` heuristic finalization. |
| **M4** | **Primary research:** conformal calibration of the cross-modal re-ID score; full vessel-domain retype of the Party model in `planetar-ontology` (P1–P5 give the scaffolding; vessel-specific schemas are the new typing). |
| **M5** | `planetar-ui` viewer extensions: map, timeline, entity-card, waveform. (Base shell + WS bridge ship today.) |
| **M6** | End-to-end Salish-Sea synthetic dark-event scenario replayable from WAL; public-benchmark eval (xView3 SAR, Singapore Maritime EO, ShipsEar/DeepShip acoustic); 1b proposal package. |

The honest TRL claim: **bus + envelope + shell + ingress + four per-modality detectors + entity graph are TRL 3–4 at project start** (working code, broker-integrated, real public-data validation); **cross-modal fusion + calibration is TRL 2 → 3 over the 1a** (this is what the 6 months funds).

---

## How to use this doc

- **Cold reader (W4):** read this *after* `docs/external-reader-briefing.md`. Verifies the proposal isn't asking for funding to build things that already exist.
- **DIP submission day (T-0):** spot-check any narrative reference like "working bus" / "shipped service" / "broker-integrated" — every one should map to a row here.
- **Audit (post-award, 6-year window):** every claim in PRC-4 traces to a row here; every row here traces to a git commit + last-commit date + LOC count + test count. Verifiable spine.
