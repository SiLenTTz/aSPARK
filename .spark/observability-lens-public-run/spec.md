# Spec: observability-lens-public-run

<!-- aspark-template: aspark/0.1.0 -->

| | |
|---|---|
| **Phase** | Specify |
| **Owner** | Product Owner (`/story-time`) |
| **Status** | `approved` |
| **Date** | 2026-10-28 |
| **Ticket** | #94 |

**Handoff**
- **Status:** `approved` (autonomous factory line; the issue's AC-1…AC-5 are the approved scope).
- **Summary:** Run the observability lens (`lenses/observability.md`) through a Review-only pass on a real public `api` pull request and commit the trail, positive and negative results alike.
- **Open:** `none`
- **Binding ruling:** §4 User Stories.
- **On conflict:** the numbered body wins except `Status`.

## 1. Problem & Goal

- **Problem:** ROADMAP names the gap: public, checkable runs. The observability lens shipped as a dry run only (0 agent sessions); ROADMAP "Next" says no public run on a real `api` exists.
- **Goal:** One re-runnable observability-lens Review on a pinned public `api` diff, each checklist item recorded fired / silent / not-verified-live with a trace.
- **Success signal:** A stranger can clone the target, check out the pinned commit, run `/peer-review` with `lenses/observability.md` and compare with `evidence.md`.

## 3. Assumptions

| # | Assumption | Resolution |
|---|---|---|
| A1 | Target has no `.spark/` trail or constitution; `api` is declared by the caller and the lens path is passed by hand. | Disclosed in evidence §2. |
| A2 | No JVM / build in this environment. | Anything needing a build is `not-verified-live`. |
| A3 | One run gives one observation, not a rate. | Disclosed. |
| A4 | Prompt material makes a check likely, never certain (CLAUDE.md). | Stated up front: the per-item trace is requested by the caller, so the run cannot show the unprompted firing rate. |

## 4. User Stories

### US-1 (Must): Name the target so it can be re-run
- [ ] AC-1: Repo, PR, merge and base commit SHA, diff command and run date are named, plus which of the lens's dispatch conditions make the target an `api`.

### US-2 (Must): Per-item record with trace
- [ ] AC-2: The Reviewer is run with the lens; `evidence.md` records per checklist item whether the lens fired, what it found, with a `file:line` trace; findings are judged right / wrong / partly against an independent author pre-read written first.

### US-3 (Must): Honest limits
- [ ] AC-3: Every limitation is disclosed as `not-verified-live` (lens passed by hand, target not built, unprompted rate not shown, Specify half not run, n = 1).

### US-4 (Must): Docs claim only what was shown
- [ ] AC-4: ROADMAP.md and docs/status.md carry the real outcome only; refuted-with-finding is valid.

### US-5 (Must): Nothing executable
- [ ] AC-5: No executable file added (constitution §3); `lenses/` untouched.

## 5. NFRs

| # | Requirement |
|---|---|
| NFR-1 | Every cited `file:line` resolves with `git show <sha>:<path>` at the pinned commit. |
| NFR-2 | Negative results are as plain as positive ones. |
| NFR-3 | Scope fence: `.spark/observability-lens-public-run/`, `ROADMAP.md`, `docs/status.md`. |

## 6. Out of Scope
UI-lens run on a website; any change to `lenses/observability.md`; the Specify half.
