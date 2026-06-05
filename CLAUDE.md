# CLAUDE.md — Kitchen OS

Guidance for Claude Code (and any AI assistant) working in this repository.

## Read this first

**`HARNESS.md` is binding.** Read it at the start of every session. It is the project's
operating contract — 10 laws, phase gates, and a Not-Yet list. Do not deviate from it. The
only legal way to break a rule is to **amend `HARNESS.md` on the record** (edit it, bump the
version, log it in the journal).

## What this is

Kitchen OS — a **Cooking Intelligence Platform first, robotics platform second**. We prove the
system can *understand* cooking (in software + cheap sensors) before spending on a robot arm.
The arm is an interchangeable actuator for later.

**Single measure of progress:** can the system watch a cooking process and correctly name the
stage it is in? Not: can a robot move.

## Current phase — Phase 0a (Kitchen OS v0.1)

Recipe text / URL → validated recipe JSON. This layer also defines the **state ontology**
(`onion_color`, `oil_temp`, …) that perception is later graded against — which is *why* it
comes first. Gate to advance: see `HARNESS.md`.

Ask every session: *"What is the smallest system that proves we understand cooking before we
automate it — and is today's work the cheapest path to the current gate?"*

## Sequencing (do not reorder without amending the harness)

Phase 0a recipe layer (₹0 hardware) → Phase 0b single auto-labeled perception spike
(**water cold→boiling** on our own induction plate) → Phase 1 broaden the state engine →
Phase 2 dataset + knowledge graph → Phase 3+ robot arm.

## Conventions

- **Language:** Python 3.12, `uv`. Python-first everywhere. Rust / TurboVec / a vector DB are
  on the Not-Yet list — introduce only on a *measured* bottleneck (Law 5).
- **Schema is the contract:** Pydantic v2 models are the source of truth for the recipe JSON.
- **LLM:** Claude (Sonnet 4.6, `claude-sonnet-4-6`) via tool-use / structured output so JSON is
  guaranteed-valid, never regex-parsed.
- **Storage:** JSON files under `recipes/` are canonical. No vector DB yet.
- **Labels have a source (Law 4):** temperature from a sensor, never inferred from RGB; discrete
  state classes before any continuous metric.

## Definition of done

Value is demonstrated through cooking-state understanding + culinary knowledge before any
significant robotics spend. Robot videos, cutting vegetables, a big bill of materials, and
speculative Rust are **not** success.

## Journaling (Law 9 — mandatory)

This repo is the single source of truth. **Every working session:** append a changelog entry at
the top of `index.html`, bump the *Last updated* / *Doc rev* stamp (hero + footer), then commit
and push. If it isn't logged, it didn't happen.

## Repo / account

GitHub: **davidfrancisloretta/kitchen-os** (public; Pages at
https://davidfrancisloretta.github.io/kitchen-os/). This project is **not** associated with the
`davidbeyondbarriers` account or the `GeorgeManjuSurvey` repo — never push Kitchen OS work there.

## Commands

Docs-only today (no code yet). When the Phase 0a pipeline lands, document its commands here
(e.g. `kitchen-os extract <url|->`).
