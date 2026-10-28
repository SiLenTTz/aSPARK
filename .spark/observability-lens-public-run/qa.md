# QA Report: observability-lens-public-run

<!-- aspark-template: aspark/0.1.0 -->

| | |
|---|---|
| **Phase** | Review (hands-on) |
| **Owner** | QA Tester (`/demo-day`) |
| **Input** | `spec.md`, `plan.md`, `evidence.md`, `review.md` (`passed`) |
| **Status** | `passed` |
| **Round** | 1 |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** `passed`.
- **Verdict:** Yes, with the disclosed limits: one prompted, partial run (evidence §2, §6 R1–R7). The docs claim no more.
- **Open:** `none`.
- **Binding ruling:** §2 table.
- **On conflict:** the numbered body wins except `Status`.

**QA method (constitution §8):** `Browser-observable surface: no`; the substitute is commands whose output was observed. Performed by the orchestrator in the session that built the trail (autonomous mode), so **not an independent tester**; rows were re-run from primary source.

## 1. Environment
No app, no browser. Commands: `git show c2cdc05:<path> | sed -n`, `git grep`, `git diff`, `git status` in `/workspace/repo` and the scratch clone.

## 2. Verification

| Spec ID | Steps | Observed | Result |
|---|---|---|---|
| AC-1 | `git rev-parse c2cdc05 c2cdc05^1`; read evidence §1 | merge `c2cdc055…`, base `34eff332…` match; dispatch facts named (controllers, port, container files, actuator) | ✅ pass |
| AC-2 | Re-resolved `ExceptionControllerAdvice.java:57,69-75,87-94`, `OwnerRestController.java:126-129,137-143`, `logback.xml:12`, `application.properties:41`, `DisableSecurityConfig.java:25`, test `:385,391`; re-ran the logger and MDC/correlation greps; counted 12 trace rows | all lines hold the described code; both greps return 0 hits; 12 rows, one status each | ✅ pass |
| AC-3 | Read evidence §2, §6, §7 | lens-by-hand, no build, unprompted rate not shown, Specify half, n=1 all stated; R1–R7 recorded | ✅ pass |
| AC-4 | `git diff ROADMAP.md docs/status.md` | both say partial; lens stays under Next; Shipped table untouched | ✅ pass |
| AC-5 | `git status --short`; `git ls-files '*.py' '*.sh' '*.js'` | changes only in `.spark/observability-lens-public-run/`, `ROADMAP.md`, `docs/status.md`; no executable; `lenses/` untouched | ✅ pass |
| NFR-1/2 | as AC-2; read §6 | citations reproduce; negatives plain | ✅ pass |

## 3. Bugs
None on the aSPARK side. Honest limit: the pre-read ordering (hash before launch) is self-reported.

## 4. Verdict
Passed as an evidence-only loop; independent QA tester not available.
