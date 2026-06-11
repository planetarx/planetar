# planetar / Zax Analytics — Moat Strategy

> **Internal company strategy.** Established 2026-05-19 in an interview with Steven Ness.
> Not for inclusion in submitted CH13 narratives — this concerns what CH13 is *for*,
> not what the proposal claims. Proposal-facing material lives in `proposal/`.

---

## The thesis in one line

The moat is a **real-time entity-resolution layer that incumbents are architecturally
locked out of** — proven by an *earned secret*, validated by a competitively-won defence
contract, and compounding through a data flywheel. **CH13 is not the goal; it is the
evidence that makes the moat fundable.**

## What CH13 is for

The follow-on to CH13 Component 1a is **a venture round** — not IDEaS Component 1b, not a
direct DND contract. CH13 is the wedge: a proof point, a first-customer signal, and
non-dilutive runway. The company is the goal.

Positioning to investors: a **defence company** (Anduril-shaped) — maritime domain
awareness as the beachhead, then other defence domains. Not a horizontal data platform.
Carry the expansion map (maritime → land / ISR → space / RF → allied MDA) so the TAM
ceiling reads as high.

---

## The moat stack

Ranked by what to bet the pitch on.

### Tier 1 — load-bearing

**1. The earned secret.**
Steven worked on Salesforce's Magic Bus + Canonical Data Architecture for Customer 360.
It was too slow to unlock what was needed. He has now built the real-time version.
Almost nobody on earth can credibly say that. This is the moat that makes a VC believe
the architecture lock-out is *intentional, not lucky* — and it is entirely non-copyable.
**Lead every pitch with it:** "I have seen this fail at the largest possible scale, and I
know precisely why."

**2. Architecture lock-out — stated precisely.**
The defensible claim is *not* "we're faster" (Palantir can hire fast engineers). It is a
**two-front** argument, and the fronts are different:

- **vs. Palantir → counter-positioning.** Foundry / Gotham's ontology sits on batch
  (Spark-class) semantics. Making it real-time is a *breaking change to their product and
  customer base*, not a feature add. They **won't**, not can't. Salesforce had infinite
  resources and still didn't crack Customer 360 on this — that is the answer to "why
  hasn't the incumbent done it."
- **vs. funded startups → integration + head start.** A startup can build a fast bus in a
  quarter. They cannot, in a quarter, build bus + envelope + ontology + five detectors +
  shell *co-designed* — *and* hold a won defence contract *and* a real-data demo. The moat
  there is the integrated vertical plus the CH13 lead time.

> Discipline: "we're faster" is not a moat. "The incumbent is structurally
> disincentivized, and competitors cannot catch the integrated head start" is.

### Tier 2 — built by CH13, compounding

**3. Ontology as system-of-record.**
The anchor artifact. Once entities live in the graph with full lineage, switching means
re-resolving history. Defence customers never rip out a system-of-record.

**4. The dark-vessel re-ID corpus / data flywheel.**
Every confirmed re-identification is a labeled example; defence-grade fusion data is
uniquely hard to obtain. *Caveat: at TRL 1–3 over one 6-month project the corpus is small.
Pitch it as "flywheel designed in, first data points collected" — never "we have a data
moat today."*

**5. Accreditation-readiness package.**
Now a real, scoped Phase-1a deliverable (evidentiary-chain spec + ATO-readiness
assessment). In defence this is a **time moat** — ATO processes run for years; a started,
documented one only you understand is a head start competitors cannot buy.

### Tier 3 — supporting, not load-bearing

- **Thin open layer.** Publish the envelope / protocol as an interoperability standard;
  keep bus internals, detectors, fusion, and ontology closed. Standard-capture at near-zero
  cost.
- **Modality-onboarding speed.** The canonical model lets a new sensor be ingested in days
  — a demo and sales asset.

---

## CH13 → venture bridge

Design every CH13 deliverable to **double as a venture asset.**

| CH13 deliverable                | Venture moat asset                                      |
|---------------------------------|---------------------------------------------------------|
| Live dark-vessel demo           | The headline slide — "it works," not "it could"          |
| Ontology / canonical data model | The system-of-record asset                               |
| Re-ID corpus                    | Flywheel evidence                                        |
| Accreditation-readiness package | "Defence-deployable" credential + time moat              |
| The won award                   | Third-party validation + first-customer signal           |
| Benchmark report                | Proof the architecture claim is measured, not marketing  |

The headline proof point for the raise is the **live dark-vessel demo** — "it works"
beats "it could."

---

## Raise timing

**Recommendation: raise on deliverables (late 2026 / early 2027); warm investors now.**

- The chosen proof point — the live demo — does not exist until the project runs. Raising
  on the award alone is raising on a promise; raising on the demo is raising on proof.
- Demo + corpus + accreditation package collectively de-risk the three questions a defence
  VC asks (does it work / does the data compound / is it deployable) → a materially better
  round, better terms, better investors.
- Pre-award is the weakest moment: solo founder, no validation, no demo.
- Investor relationships take 6–9 months to warm — so start *conversations* now (not a
  process). Use the mid-2026 CH13 award as a natural milestone touchpoint; open the actual
  round once the demo is live.

**This recommendation flips if runway is the constraint.** If CH13's ≤$250K CAD does not
cover the founder through delivery, a small angel / pre-seed bridge may be needed sooner.
*Open question: does CH13 funding cover runway through delivery, or is there a gap?*

---

## Where the moat is thin (say it before a VC does)

- **No relationships, no clearance.** The moat is all-technical. If a prime *does* commit
  to the real-time rebuild, the only defence is speed + head start. Move fast; the
  accreditation head start partly offsets.
- **Solo-founder key-person risk.** The earned secret offsets it (the moat *is* the
  founder) but a credible team plan is needed.
- **"Defence company" caps the TAM story.** Chosen deliberately — fine — but carry the
  expansion map so investors see the ceiling is high.
- **Canadian-controlled status** is a moat for *Canadian* defence contracts but a
  *constraint* on US capital (a16z American Dynamism, Founders Fund) and ITAR / CGP.
  Decide early.

---

## Three next-level moves

1. **Instrument the demo to *generate* the corpus.** Every run on live Victoria AIS
   produces labeled re-ID events. The demo is not just proof — it is the flywheel's first
   turn. Make that visible to investors.
2. **Write the "why Palantir won't" memo now.** It is simultaneously the spine of the
   investor narrative *and* PRC-2 (novelty / 20 pts). One artifact, two payoffs.
3. **Land a second non-DND design partner** (port authority, Transport Canada, a
   fisheries body). Proves modality-onboarding speed, seeds a second corpus, and blunts the
   single-buyer concern of a defence-only pitch.

---

## Decisions on record (interview, 2026-05-19)

| Question                          | Decision                                                        |
|-----------------------------------|------------------------------------------------------------------|
| Moat horizon                      | CH13 contract moat — engineer the follow-on                      |
| Threat to defend against          | Big primes (Palantir) **and** funded defence-tech startups       |
| Moats to bet on                   | Cross-modal correlation IP, provenance/accreditability, ontology |
| OSS posture                       | Thin open layer — envelope/protocol public, rest closed          |
| Win vs. lock-in                   | Engineer the follow-on                                           |
| Correlation edge                  | Architecture-locked (latency unlocks the capability class)       |
| Provenance status                 | Was "just framing" → now a real Phase-1a deliverable             |
| Hidden relationship/credential    | None (no ONC link, no DND champion, no clearance)                |
| What company is being pitched     | A defence company                                                |
| Anti-Palantir mechanism           | The real-time bus underneath the ontology                        |
| Headline proof point              | The live dark-vessel demo                                        |
| Raise timing                      | Advised: on deliverables, warm investors now                     |
