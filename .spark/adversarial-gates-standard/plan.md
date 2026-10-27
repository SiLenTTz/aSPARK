# Plan: adversarial-gates-standard

| | |
|---|---|
| **Status** | `approved` |
| **Source** | parent plan `.spark/adversarial-gates/plan.md` D7, T10–T15, re-cut below |

Decisions D7/D8 of the parent plan stand: `standards/anti-generosity.md` (≤40 lines), literal-path dispatch in step 2 of both skills, no template/agent/constitution edit, exact cell `⚠ not-verified-live`. Both claimed phases (review, qa) checked for dispatch and activation (AC-2.6).

| # | Task | Covers | Status | DoD |
|---|---|---|---|---|
| T10 | Negative baseline | AC-2.6, NFR-2 | `done` | grep of `skills agents templates lenses` for `anti-generosity\|standards/` empty before any edit |
| T11 | Standard file | AC-2.1–2.5, 2.9 | `done` | ≤40 lines, frontmatter, corpus citations |
| T12 | Dispatch in both skills | AC-2.5, 2.8, NFR-1, NFR-3 | `done` | ≤12 added lines per skill, literal path |
| T13 | Activation/dispatch proof | AC-2.5–2.8 | `done` | structural grep + live runs `⚠ not-verified-live` |
| T14 | Verdict-behaviour dry runs | AC-2.1–2.4 | `done` | `⚠ not-verified-live` (no `claude` CLI) |
| T15 | Docs, version, audits | AC-4.6, NFR-4–7 | `done` | README, ROADMAP, status, repo-layout; 0.15.0 |

Gate: spec approved; plan approved (the parent plan carried the user's approval of D7/D8; this re-cut changes no decision).
