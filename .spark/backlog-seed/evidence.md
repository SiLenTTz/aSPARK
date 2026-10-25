# Evidence: backlog-seed

Dry run for constitution §4 (review round 1, F2). Nothing was filed; `gh` is not installed in this environment, so step 7 (filing) was never run.

## Runs

| # | Case | Method | Result |
|---|---|---|---|
| E1 | Negative: no `docs/` and no constitution | Read SKILL.md step 1 | Text stops at `/charter`, writes nothing. Read only, not run as a session. |
| E2 | Positive, steps 1-2: Product Owner proposes from this repo's `docs/`, constitution and `ROADMAP.md` | Real `product-owner` agent run, no files written | 6 items, each with a source that exists. It linked items that already have issues (#8, #13, #14, #15) and left out items that are not buildable here. One run, so no rate. |
| E3 | Shell safety of the step 7 shape (F1) | Heredoc probe: body with `` `id -un` `` and `$(echo INJECTED)` fed via `<<'EOF'` | Printed literally, nothing executed. A single-quoted title with `'` written as `'\''` also kept the text literal. |
| E5 | Negative precondition: empty scratch dir with no `docs/`, no constitution, no ROADMAP | `ls` in `/tmp/scratch-neg` | All three absent. Only the precondition was run. No session followed step 1 there, so the `/charter` stop and "writes nothing" are read from the text, as in E1 (review r3, F2). |
| E6 | Positive, steps 1, 2, 5, 6 (review round 2) | Step 1 gathered headings by `grep` (no lead paragraphs). Step 2: a fresh `product-owner` agent got the headings, the constitution as a path, ROADMAP headings and a cap of "at most 6 items". The brief also said "Read the files only as needed", and the agent read `ROADMAP.md` and grepped `docs/status.md`, against `product-owner.md:196` (review F23). Step 3: the list was **not shown** to the user; the source check is not visible in the transcript, and the reviewer re-checked all six sources against the files at r3. Step 4 never ran. Step 5: `origin` = `a-lottes/aSPARK`. Step 6: the user was asked via the question tool | 6 items, one-letter sizes (S/M/L), dependencies on items in the list, sources all existing files and headings (two cite `ROADMAP.md#next`). The user answered **no** at step 6 without seeing the list, so nothing was filed. One run, no rate. |
| E4 | Step 7 filing, `gh` failure path | `command -v gh` returned nothing | Not verified live. Filing to a real or scratch tracker never ran. |

## AC table

| AC | Evidence |
|---|---|
| AC-1.1 | E2, E6 (ROADMAP handed over, F3) |
| AC-1.2 | E1, E5 (precondition run, the stop read only) |
| AC-2.1, 2.3 | E6 output (E2 had "S–M" sizes in two items, so it is not counted; both briefs let the agent read files, F23) |
| AC-2.2 | E2, E6: every proposed source existed. Best effort, no rate measured |
| AC-3.1 | E6, half: confirmation asked and declined, but the list was never shown as a plan |
| AC-3.2, 5.x | Read against the text, not run |
| AC-4.1, 4.2 | E3 (command shape safe); filing itself not run (E4) |
| AC-4.3 | Read against the text, not run |
