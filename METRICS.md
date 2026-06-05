# Kitchen OS — Metrics & Tracking

**v1 · 2026-06-05 18:27 IST** · supports [`PLAN.md`](PLAN.md)

How we track progress across the SDLC. Two layers: **DORA** (software-delivery performance —
the scaffold we instrument from day one) and **Kitchen-OS leading indicators** (what actually
signals progress this early). Update the running table each iteration.

> *Note:* DORA measures **delivery** performance. Until we ship code regularly it stays low —
> but our daily journal/Pages pushes already exercise the pipeline, so we instrument it now and
> grow into it. Early on, the **leading indicators** below are the real progress signal.

---

## 1. DORA — the four keys

| Metric | Definition | How we measure | Early target |
|---|---|---|---|
| **Deployment Frequency** | how often we ship to a live target | count of GitHub Actions deploys / Pages pushes per week | ≥ 3 / week |
| **Lead Time for Changes** | commit → live | timestamp(commit) → timestamp(deploy) via Actions | < 1 day |
| **Change Failure Rate** | % of deploys causing a failure/rollback | failed-deploy or revert count ÷ deploys | < 15 % |
| **MTTR** | time to restore after a failed change | incident open → resolved | < 1 day |

---

## 2. Flow metrics

| Metric | Definition | Target |
|---|---|---|
| Throughput | recipes codified / recordings captured per week | trend ↑ |
| Cycle time | issue start → merged | < 3 days |
| WIP | items in progress at once | ≤ 2 (solo founder) |

---

## 3. Kitchen-OS leading indicators *(the ones that matter now)*

| Metric | Definition | Phase | Target |
|---|---|---|---|
| **Schema-valid rate** | % extractions that pass Pydantic | 0a | 100 % |
| **Gold-step accuracy** | % gold steps with correct `action`+`ingredient` | 0a | ≥ 90 % |
| **Observable coverage** | % state-steps that name an `observable` | 0a | 100 % |
| **₹ / recipe** | LLM cost per codified recipe | 0a+ | trend ↓ |
| **Recordings captured** | # sensor-grounded recordings | 0b | trend ↑ |
| **Sensor-sourced values** | % recipe-file values from a sensor (not eyeballed) | 0b | 100 % |
| **Recreation fidelity** | 1 − normalized error vs target profiles (weight, temp, dT/dt, colour, duration) | 0c | ≥ agreed % |
| **Time-to-first-faithful-recreate** | days from recording → passing recreate | 0c | trend ↓ |

---

## 4. Quality metrics

| Metric | Definition | Target |
|---|---|---|
| Test coverage | line/branch coverage of `kitchen_os/` | ≥ 80 % core |
| Defect escape | bugs found after a gate ÷ total | trend ↓ |
| Lint/type clean | `ruff` + `mypy` pass on CI | 100 % |

---

## 5. Running log *(append each iteration)*

| Date | Iter | Deploy freq (wk) | Lead time | Schema-valid | Gold-step acc | Fidelity | Notes |
|---|---|---|---|---|---|---|---|
| 2026-06-05 | Plan | ~6 (docs, today) | ~minutes | n/a | n/a | n/a | Pre-code; docs pipeline live (Pages). DORA scaffold confirmed working. |

*Baseline is intentionally mostly n/a — we are pre-code. The point is the instrument exists
before the code does, so progress is visible from the first build.*
