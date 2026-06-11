# Demo handoff — two-Operation VC demo (Victoria + Strait of Hormuz)

Written 2026-05-19. Scope: how to bring up the planetar stack so a VC demo
can run **two browser tabs side by side** — one tracking Victoria, BC
traffic, one tracking the Strait of Hormuz tanker blockage.

The code changes are done and verified. This file is for the agent
handling the **servers / process orchestration**.

---

## The goal

| Tab | Operation | Server rail tile | What it shows |
|---|---|---|---|
| 1 | Pacific Patrol | **PP** | Victoria, BC fleet — Juan de Fuca / Haro Strait |
| 2 | Strait of Hormuz | **SH** | Congested tanker anchorage off Bandar Abbas + shadow-fleet dark tankers |

Each tab's map is scoped to its own Operation — vessels from the other
Operation do not bleed through.

---

## What has to be running

| Process | Where | Port / endpoint | Notes |
|---|---|---|---|
| planetar-broker | `~/github/planetarx/planetar-broker` | TCP 12001 (pub) / 12002 (sub) / 12003 (udp) | The C bus. Build with `make`, run `./planetar-broker`. |
| planetar-ais (Victoria) | `~/github/planetarx/planetar-ais` | → broker 12001 | `npm start` — default `OP=victoria`. |
| planetar-ais (Hormuz) | `~/github/planetarx/planetar-ais` | → broker 12001 | `OP=hormuz npm start` — **second process, same repo, same broker.** |
| planetar-ui bridge | `~/github/planetarx/planetar-ui` | WS `ws://127.0.0.1:9100` | TCP↔WS proxy. `npm run dev:bridge`. |
| planetar-ui (Vite) | `~/github/planetarx/planetar-ui` | `http://127.0.0.1:5180` | `npm run dev:ui`. |

**One planetar-ais process serves exactly one Operation.** The two-tab
demo therefore needs **two** planetar-ais processes against the same
broker — that is the key thing that is easy to miss.

---

## Bring-up order

```bash
# 1. Broker — the bus everything else connects to.
cd ~/github/planetarx/planetar-broker
make            # first time only
./planetar-broker

# 2. AIS feed — Victoria (default Operation).
cd ~/github/planetarx/planetar-ais
npm install     # first time only
npm start

# 3. AIS feed — Strait of Hormuz (second process, new terminal).
cd ~/github/planetarx/planetar-ais
OP=hormuz npm start

# 4. UI bridge + Vite dev server (new terminals, planetar-ui).
cd ~/github/planetarx/planetar-ui
npm install     # first time only
npm run dev:bridge
npm run dev:ui
```

`npm run dev` (in planetar-ui) starts vite + bridge + a synthetic-chatter
process together. For a clean demo driven only by the broker + AIS, run
`dev:bridge` and `dev:ui` separately and **skip `dev:synth`** so the only
traffic is the real fleets.

Expected startup log lines from the AIS processes:

```
[planetar-ais] op=victoria (Pacific Patrol) server=pac source=mock broker=127.0.0.1:12001
[planetar-ais] op=hormuz (Strait of Hormuz) server=hormuz source=mock broker=127.0.0.1:12001
[publisher] connected to broker 127.0.0.1:12001
```

---

## Setting up the two browser tabs

1. Open `http://127.0.0.1:5180` in **tab 1** → click the **PP** tile in
   the left server rail (Pacific Patrol). Map settles on Victoria.
2. Open the same URL in **tab 2** → click the **SH** tile (Strait of
   Hormuz). Map flies to the Persian Gulf chokepoint.

Vessels stream in over ~7 s as each fleet announces (`appeared` events
are staggered). Hormuz seeds 20 vessels — 14 anchored tankers forming the
blockage, plus shadow-fleet tankers that periodically drop AIS.

### Demo caveat — shared localStorage

Both tabs share the same browser origin, so they share `localStorage`.
The UI reads the persisted Operation/channel **only on page load** and
writes on every change. Consequences:

- Set the tabs up once and **do not reload them mid-demo** — a reload
  makes the tab pick up whichever Operation was selected last (likely the
  *other* tab's).
- If a tab must be reloadable, run the two tabs in **separate browser
  profiles** (or one normal + one incognito window) so they get
  independent `localStorage`.

---

## What changed in the code (already done, for context)

**planetar-ais**
- `src/areas.mjs` — new. Operating-area registry: `victoria` (server
  `pac`) and `hormuz` (server `hormuz`), each with bbox / center /
  anchorage / seed fleet.
- `src/source-mock.mjs` — area-aware; seed flags `anchored` (clusters at
  the anchorage at low speed — the blockage) and `darkProne` (drops AIS
  often — shadow-fleet behaviour).
- `src/index.mjs` — `OP` env var selects the area; `vessel.ais.position`
  payloads now carry `serverId` so the UI can scope the map.
- `src/source-aisstream.mjs` — subscribes to the selected area's bbox.

**planetar-ui**
- `src/data/mocks.ts` — new `hormuz` server (`SH` tile) + 8 channels;
  each server carries a `center` (map home).
- `src/store/aisStore.ts` / `src/types/ais.ts` — vessel records carry
  `serverId`.
- `src/components/tabs/MapTab.tsx` — map vessel layer is filtered to the
  active Operation; camera recentres on the Operation's home on switch.
- `src/store/layoutStore.ts` — switching Operations clears vessel
  selection.

---

## Verification checklist

- [ ] Broker accepts connections on 12001 (AIS `publisher connected` log).
- [ ] Two AIS processes running — log lines show `op=victoria` and `op=hormuz`.
- [ ] Bridge reachable at `ws://127.0.0.1:9100`; UI shows it in "real" mode.
- [ ] PP tab: Victoria vessels on the map, map over Juan de Fuca.
- [ ] SH tab: Hormuz tanker cluster on the map, map over the Strait of Hormuz.
- [ ] Neither tab's map shows the other Operation's vessels.

---

## Optional — real AIS instead of the mock fleet

The mock fleet is deterministic and demo-safe. To drive an Operation from
live aisstream.io data instead:

```bash
export AISSTREAM_API_KEY=...        # free key at aisstream.io
AIS_SOURCE=aisstream OP=hormuz npm run start:aisstream
```

The subscription bbox follows `OP`. Real Hormuz traffic is heavy and
unpredictable — for a controlled VC demo the mock fleet is recommended.

See `planetar-ais/README.md` for the full env-var table and
`planetar-ui/HANDOFF-AIS.md` for the AIS→UI data path.
