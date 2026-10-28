# Plan: website-lens-public-run

<!-- aspark-template: aspark/0.1.0 -->

| | |
|---|---|
| **Phase** | Plan |
| **Owner** | Engineering Manager (`/sprint-plan`), drafted by the Factory line in dark mode |
| **Input** | `.spark/website-lens-public-run/spec.md` (`approved`) |
| **Status** | `approved` |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** `approved` (dark mode, recommended gate).
- **Summary:** Profile the target in a constitution, dispatch the accessibility lens from it to a `reviewer` agent (no trace request) and a `qa-tester` agent (live browser), record results and update the docs.
- **Open:** `none`
- **Binding ruling:** §3 Task Breakdown.
- **On conflict:** body wins except `Status`.

## 1. Architecture Decision

- **Decision:** Target `https://github.com/StartBootstrap/startbootstrap-agency` at `b2d5d5c6b1396bc5b0e8dd92c693b106cfb7cee1`: a static MIT page with a modal, a collapsing navbar and a form, so every §4/§5 lens item has something to bite on, and `dist/` serves with `python3 -m http.server` (no build). The clone and its constitution live in untracked scratch `/tmp/target/`. The reviewer reads the snapshot diff against the empty tree. The QA tester gets the running URL.
- **Alternatives considered:**

| Alternative | Why rejected |
|---|---|
| Run on this repo | It has no UI (constitution §2); the lens would be silent. |
| A site needing a build or backend | Not reproducible inside the container. |
| Pass the lens by hand | The api run did that; AC-2 exists to close it. |

- **Consequences:** No real increment, so Act is not exercised. Profile is assumed. n = 1.

## 2. Affected Components

`.spark/website-lens-public-run/*`, `ROADMAP.md`, `docs/status.md`. Scratch: `/tmp/target/startbootstrap-agency`.

## 3. Task Breakdown

| # | Task | Story | Status | Definition of Done | Covers (AC / NFR) | Depends on |
|---|---|---|---|---|---|---|
| T1 | Clone, pin, write and install the target constitution, serve `dist/` | US-1, US-2 | done | `curl`-free HTTP 200 check on `http://127.0.0.1:8766/`; constitution copied | AC-1, AC-2 | none |
| T2 | `/peer-review` via `reviewer` with the lens resolved from the constitution; no trace request | US-2, US-3 | done | `review.md` written by the agent | AC-2, AC-3 | T1 |
| T3 | `/demo-day` via `qa-tester` in the browser with the lens resolved the same way | US-4 | todo | `qa.md` with PASS/FAIL + observable per lens check | AC-4, NFR-1, NFR-2 | T2 |
| T4 | `evidence.md`: target, activation, firing, live checks, not shown, misfires | US-1…US-5 | todo | Citations re-resolved at the pinned commit | AC-1…AC-5 | T3 |
| T5 | Update `ROADMAP.md` and `docs/status.md` to the result only | US-5 | todo | Wording matches evidence | AC-5, AC-6, NFR-4 | T4 |

## 4. Test Strategy

Re-resolve every cited line with `git show b2d5d5c:<path> | sed -n '<n>p'`; `git status` shows changes only under the fence.

## 5. Risks

- The agents may skip the lens (a result, recorded).
- CDN reachability in the container (A5): read the network log.
- Prompt contamination: the reviewer prompt is written without lens item names.
