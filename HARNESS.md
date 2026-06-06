# Kitchen OS — Project Harness

**v2 · 2026-06-05 18:47 IST**

This is the operating contract for Kitchen OS. Its job is to keep us building the
*one* thing that matters and to make drift impossible-by-accident. Read it at the
**start of every planning or build session**.

> **The only legal way to break a rule in here is to amend this file on the record.**
> You may change the harness deliberately (edit it, bump the version, log it in the
> journal) — but you may **not** deviate from it silently. Drift is a bug.

---

## North Star

> Build a **Cooking Intelligence Platform** that can *understand* a cooking process —
> proven in software and cheap sensors — **before** spending meaningfully on robotics.
> The robot arm is an interchangeable actuator bolted onto an already-intelligent system.

**The single measure of the North Star:** can we **record a dish once and have a guided
operator (who can't cook) recreate it to within an agreed X% profile fidelity** — no robot?
Not: can a robot move.

State-understanding is **not** dropped — *"can the system name the cooking state?"* is an
**early rung** on the ladder to that North Star. We climb progressively; each rung is a
milestone, and many steps sit between the first rung and the last.

### Milestone Ladder *(progressive — climb in order)*

| # | Milestone | Phase |
|---|---|---|
| M1 | **Codify from text** — recipe → valid structured Recipe File (text→JSON) | 0a |
| M2 | **Record one cook** — capture synchronized sensor streams for one dish | 0b |
| M3 | **Name the cooking state** — from the signals, correctly call the stage (e.g. water boiling, onion golden) as discrete, sensor-grounded classes | 0b–1 |
| M4 | **Codify a recording** — fit real sensor profiles into a sensor-grounded Recipe File | 0b–0c |
| M5 | **Guided recreate (open loop)** — a human reproduces the dish via step guidance + weight validation | 0c |
| M6 | **Score fidelity** — actual vs target profiles → a fidelity % | 0c |
| M7 | **Closed-loop recreate** — real-time adjustment to hit the recorded targets | 1 |
| ★ | **North Star — record once, recreate within X% fidelity** (multiple dishes; operator who can't cook) | 1+ |

---

## The Laws

1. **Intelligence before actuation.** We do not buy, build, or design a robot arm until
   the perception + recipe layers can demonstrably understand a cooking process. The arm
   is the *last* thing we touch, not the first.

2. **Cheapest falsifiable experiment first.** Always pick the smallest test that can prove
   or kill the current assumption. ₹0/days beats ₹lakh/months. "Prove the hardest part
   first" is **banned**; "prove the cheapest informative part first" is the rule.

3. **No spend before a passed gate.** No hardware, no paid tools, no new infrastructure
   until the prior phase gate is met with its written, measurable criterion (see Gates).

4. **Every label has a source.** No success metric may depend on data we can't ground.
   **Temperature comes from a sensor, never inferred from RGB.** Continuous claims
   ("75% browned") are forbidden until we have labeled data that justifies regression —
   use **discrete classes** (raw / translucent / golden / burnt) until then.

5. **Python-first; Rust only on a proven bottleneck.** Every layer starts in Python. We
   introduce Rust, a streaming service, or TurboVec/a vector DB **only** when a *measured*
   throughput or latency problem demands it — never speculatively. We never rewrite the AI
   ecosystem in Rust.

6. **Scope is subtractive.** The default answer to "should we also build X?" is **"not yet."**
   New ideas go on the *Not-Yet List* with a named trigger; they do not enter the current
   phase.

7. **Build the moat, not the demo.** Optimise for reusable cooking intelligence — data,
   ontology, models — not for impressive one-off robot videos. Success is **not** measured
   by robotic manipulation.

8. **Data flywheel by construction.** Wherever possible, instrument the work so that *doing
   it generates labeled data automatically* (sensors as auto-labelers). Manual labeling is
   a last resort, not a plan.

9. **One source of truth.** This repo (`davidfrancisloretta/kitchen-os`) is the canonical
   record. Every working session appends a journal changelog entry and bumps the timestamp.
   **If it isn't logged here, it didn't happen.**

10. **Falsify on a clock.** Every phase has a time + cost box. If the gate isn't met inside
    it, we **stop and re-decide** — we do not grind. There are no open-ended phases.

---

## Phase Gates

Each phase must **pass its gate** before any spend on the next. The gate is the only thing
that unlocks the next phase.

### Phase 0a — Recipe Intelligence *(Kitchen OS v0.1)* — **current**
Text / URL → validated recipe JSON. This layer also **defines the state ontology**
(`onion_color`, `oil_temp`, …) that perception is later graded against — which is *why* it
comes first.
- **Box:** ~2 weeks · ₹0 hardware.
- **Gate to advance:** 100% of outputs schema-valid · ≥90% gold-step `action`+`ingredient`
  accuracy on ~20 recipes · every `state` step names an `observable`.

### Phase 0b — Single-Transition Perception Spike
One webcam + one thermocouple over **our own induction plate**. Prove **one** state
transition — **water cold → boiling** — with the thermocouple + audio **auto-labeling** the
ground truth. One food, one axis.
- **Box:** ~3 weeks · ≤ ₹15,000 hardware.
- **Gate:** detector calls the transition correctly on N held-out cooking sessions ≥ X%,
  with every label grounded by a sensor (not eyeballed).

### Phase 1 — State Engine Breadth
Add onion (raw→golden→burnt), oil (cold→ready→smoking), curry (watery→reducing→finished),
each as **discrete** classes.
- **Gate:** reliable state calls across ≥3 domains on our own kitchen data. Only after 0b.

### Phase 2 — Dataset & Knowledge Graph *(milestone, not MVP)*
Begin the *Indian Cooking Foundation Dataset* via sensor auto-labeling + transcript-aligned
YouTube weak labels; build recipe graphs.
- **Gate:** auto-label loop proven on our own kitchen before any scale-up.

### Phase 3+ — Robot Execution *(future)*
Cheap arm (RoArm M2 / SO-100 / Lite 6) as an actuator on the already-intelligent system.
First task is tiny: *add onions to a hot pan and stir 60s.*

---

## The Not-Yet List *(banned until the named trigger fires)*

| Thing | Unlocks when… |
|---|---|
| Robot arm (any) | the State Engine reliably reads ≥3 cooking domains |
| Rust sensor gateway / event-streaming service | a *measured* event rate Python can't handle |
| TurboVec / dedicated vector DB | the recipe/embedding corpus outgrows flat files + in-memory search |
| Foundation dataset at scale (10k videos) | the auto-label loop is proven on our own kitchen |
| Culinary knowledge graph | we have ≥X grounded recipes on a consistent ontology |
| Thermal camera (FLIR Lepton) | RGB + thermocouple is proven insufficient for a needed state |
| Web dashboard / app / cloud backend | a second person actually needs to use it |
| Knife / cutting work · gas stove | — **not in scope** for the foreseeable prototype |

---

## The Gate Question *(ask out loud every session)*

> **"What is the smallest system that proves we understand cooking before we attempt to
> automate it — and is today's work the cheapest path to the current gate?"**

If today's task isn't on the shortest path to the current phase gate, it is a distraction.

---

## Definition of Done *(project level)*

The company's value comes — demonstrably — from **cooking-state understanding and culinary
knowledge**, before any significant robotics spend. Anything that doesn't move that is a draft.

## Success is NOT

Robot videos · cutting vegetables · a fully automated kitchen · a big bill of materials ·
lines of Rust · a clever architecture diagram. **Only state understanding counts.**

---

**Amendments:** v2 (2026-06-05 18:47 IST) — North Star set to *record → recreate within X%
fidelity*; added the **Milestone Ladder** (state-naming is rung **M3**, early — not dropped).
Logged in journal r6.

*Amend deliberately. Log every change. Never drift.*
