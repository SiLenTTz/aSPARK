# Evidence: backlog-seed

Dry run for constitution §4 (review round 1, F2). Nothing was filed; `gh` is not installed in this environment, so step 7 (filing) was never run.

## Runs

| # | Case | Method | Result |
|---|---|---|---|
| E1 | Negative: no `docs/` and no constitution | Read SKILL.md step 1 | Text stops at `/charter`, writes nothing. Read only, not run as a session. |
| E2 | Positive, steps 1-2: Product Owner proposes from this repo's `docs/`, constitution and `ROADMAP.md` | Real `product-owner` agent run, no files written | 6 items, each with a source that exists. It linked items that already have issues (#8, #13, #14, #15) and left out items that are not buildable here. One run, so no rate. |
| E3 | Shell safety of the step 7 shape (F1) | Heredoc probe: body with `` `id -un` `` and `$(echo INJECTED)` fed via `<<'EOF'` | Printed literally, nothing executed. A single-quoted title with `'` written as `'\''` also kept the text literal. |
| E4 | Step 7 filing, `gh` failure path | `command -v gh` returned nothing | Not verified live. Filing to a real or scratch tracker never ran. |

## AC table

| AC | Evidence |
|---|---|
| AC-1.1 | E2 (ROADMAP now handed over, F3) |
| AC-1.2 | E1 (read only) |
| AC-2.1, 2.3 | E2 |
| AC-2.2 | E2: every proposed source existed. Best effort, no rate measured |
| AC-3.1, 3.2, 5.x | Read against the text, not run |
| AC-4.1, 4.2 | E3 (command shape safe); filing itself not run (E4) |
| AC-4.3 | Read against the text, not run |
