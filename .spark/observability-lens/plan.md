# Plan: observability-lens

| | |
|---|---|
| **Status** | `approved` |

Architecture: add-a-file. Type-triggered (`applies-to: [api]`), no new characteristic, so no change to the Characteristics table. Rejected: a new `is-deployed-service` characteristic (a second activation path for a single lens, and `api` already means a deployed backend). Rejected: a `plan`/`qa` phase (`/sprint-plan` dispatches no lens files; no browser surface).

| # | Task | Status |
|---|---|---|
| T1 | `lenses/observability.md` | done |
| T2 | Registry: `lenses/README.md` table + type bullet + `api` signal row; facilitator + constitution template mapping | done |
| T3 | Docs: README, ROADMAP #15, `docs/status.md`, `docs/workflow.md`, `docs/family.md`, `plugin.json` 0.15.0 | done |
| T4 | Negative-case row in `.spark/constitution.md`; evidence | done |
| T5 | Positive dry run | done |
