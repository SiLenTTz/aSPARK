# Evidence: backlog-seed

Dry run for constitution §4 (review round 1, F2). Nothing was filed; `gh` is not installed in this environment, so step 7 (filing) was never run.

## Runs

| # | Case | Method | Result |
|---|---|---|---|
| E1 | Negative: no `docs/` and no constitution | Read SKILL.md step 1 | Text stops at `/charter`, writes nothing. Read only, not run as a session. |
| E2 | Positive, steps 1-2: Product Owner proposes from this repo's `docs/`, constitution and `ROADMAP.md` | Real `product-owner` agent run, no files written | 6 items, each with a source that exists. It linked items that already have issues (#8, #13, #14, #15) and left out items that are not buildable here. One run, so no rate. |
| E3 | Shell safety of the step 7 shape (F1) | Heredoc probe: body with `` `id -un` `` and `$(echo INJECTED)` fed via `<<'EOF'` | Printed literally, nothing executed. A single-quoted title with `'` written as `'\''` also kept the text literal. |
| E5 | Negative, run: empty scratch dir with no `docs/`, no constitution, no ROADMAP | `ls` in `/tmp/scratch-neg`, then SKILL.md step 1 applied | All three absent, so the skill stops at `/charter` and writes nothing. The stop itself is the orchestrator following the text, not a separate session. |
| E6 | Positive, steps 1-6 run in order, round 2 | Step 1 gathered headings by `grep`; step 2 brief per SKILL.md given to a fresh `product-owner` agent (excerpts, no "read the files" instruction beyond need); step 3 checked by hand; step 5 repo resolved from `origin` = `a-lottes/aSPARK`; step 6 asked the user via the question tool | 6 items, one-letter sizes (S/M/L), sources all existing files and headings (two cite `ROADMAP.md#next`). User answered **no** at step 6, so nothing was filed. One run, no rate. The E2 run's "S–M" sizes did not recur here. |
| E4 | Step 7 filing, `gh` failure path | `command -v gh` returned nothing | Not verified live. Filing to a real or scratch tracker never ran. |

## AC table

| AC | Evidence |
|---|---|
| AC-1.1 | E2, E6 (ROADMAP handed over, F3) |
| AC-1.2 | E1, E5 |
| AC-2.1, 2.3 | E6 (E2 had "S–M" sizes in two items and a brief that told the agent to read the files itself, so it is not counted) |
| AC-2.2 | E2, E6: every proposed source existed. Best effort, no rate measured |
| AC-3.1 | E6 (step 6 confirmation asked, declined) |
| AC-3.2, 5.x | Read against the text, not run |
| AC-4.1, 4.2 | E3 (command shape safe); filing itself not run (E4) |
| AC-4.3 | Read against the text, not run |
