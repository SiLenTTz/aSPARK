# Plan: observability-lens-public-run

<!-- aspark-template: aspark/0.1.0 -->

| | |
|---|---|
| **Phase** | Plan |
| **Owner** | Engineering Manager (`/sprint-plan`) |
| **Input** | `.spark/observability-lens-public-run/spec.md` (`approved`) |
| **Status** | `approved` |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** `approved` (autonomous, recommended gate).
- **Summary:** Review-only lens run on spring-petclinic-rest PR #302 (merge `c2cdc05`), Reviewer agent blind to the author's pre-read.
- **Open:** `none`
- **Binding ruling:** §3.
- **On conflict:** body wins except `Status`.

## 1. Architecture Decision

- **Decision:** Reuse the `api-lens-public-run` pattern. Target PR #302 (hide technical error details): it rewrites an error handler, so the observability question "is the failure still recorded server-side?" has something to bite on. Clone to untracked scratch; Reviewer gets the diff range, the lens path and a trace request, not the pre-read.
- **Alternatives rejected:** PR #329 again (already used by the api-lens run; a different diff widens evidence); a dependabot bump (lens silent everywhere); building the target (no JVM).
- **Consequences:** items 3, 4, 6 will likely be silent (diff touches no health/metrics/tracing); that is a result, not a defect.

## 2. Affected Components
`.spark/observability-lens-public-run/*`, `ROADMAP.md`, `docs/status.md`. Scratch: `/tmp/target/spring-petclinic-rest`.

## 3. Task Breakdown

| # | Task | Story | Status | Definition of Done | Covers | Depends on |
|---|---|---|---|---|---|---|
| T1 | Pin target; author pre-read written before the review | US-1 | done | pre-read file exists with sha256 before reviewer launch | AC-1, NFR-1 | none |
| T2 | Run Reviewer agent with the observability lens | US-2 | done | `review.md` written by the agent | AC-2 | T1 |
| T3 | Compile `evidence.md` | US-2, US-3 | done | per-item trace, right/wrong/missed, limits | AC-2, AC-3, NFR-2 | T2 |
| T4 | Update ROADMAP.md and docs/status.md | US-4, US-5 | done | wording matches evidence | AC-4, AC-5, NFR-3 | T3 |

## 4. Test Strategy
Re-resolve every citation with `git show c2cdc05:<path> | sed -n`; `git status` shows only fenced paths; no executable files.

## 5. Risks
Reviewer may ignore the lens (a result). Pre-read ordering is self-reported.
