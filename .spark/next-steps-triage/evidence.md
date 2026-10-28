# Evidence: next-steps-triage

Reduced run versus plan T2-T4 (autonomous line, no remote, no `gh`): **1 session per fixture, not 3**, product-owner agent in propose mode, brief only. Rates are 1/1, i.e. anecdotal.

| Fixture | AC | Expected | Observed (first line / JSON) | Result |
|---|---|---|---|---|
| F1 greenfield, 0 issues, 3 functions | AC-1 | `foundation`, 3 gaps w/ source | `foundation` walking skeleton; 3 gaps cite constitution §4 (a)(b)(c); JSON has no `existing_issue` | 1/1 |
| F2 #5 blocked by #4 | AC-2 | `existing #4` | `existing #4`, `existing_issue: 4` | 1/1 |
| F3 X has issue, Y none | AC-3 | Y gap, X not | audit log gap (constitution §4 + ROADMAP); CI not a gap; picked `existing #9` | 1/1 |
| F4 issue with merged PR | AC-4 | "done, close it" | #7 listed under done, close it; #11 picked | 1/1 |

Not run: T2 (real `/next-steps` with no tickets, AC-5), T4 (live run), skill-level brief gathering. AC-5 is verified by reading only (`skills/next-steps/SKILL.md`, ticket bullet). AC-6: field defined in SKILL.md step 4 and `docs/workflow.md`; the Factory's `proposals.py` is in another repo and was not tested.

Prompt version: these four sessions ran before commit `6b10800`, i.e. before the review r1 edits to `agents/product-owner.md` (propose-mode intro, F10; unreachable-tickets line in step 0, F7). Neither edit touches a path these fixtures exercised; they were not re-run.

User rulings (review r2, relayed by the `/peer-review` caller): Q1 / review F1 kept: the user waives constitution §6 silent fallback for this one line, reason "issue #110 asks for it". Review F3 waived: 1 session per fixture, no-ticket and live paths not run, disclosed here, in `docs/status.md` and `spec.md`. Q2 stays an agent ruling; it picked the §6-conformant option, so no waiver is involved.

Rulings (autonomous, plan-time, superseded for Q1 by the user ruling above): Q1 keep the single "Could not see the tickets" line (issue AC-5 asks for it; exception to the constitution's silent fallback, flagged for `/charter`). Q2 emit the JSON only when `agent.yaml` exists or the caller asks.

D1-D4 fixed: brief list extended, thin-brief step ref kept at step 6 after the new step became step 0, maturity step placed before ranking, unreachable tickets skip triage/gaps.

F4 ruling (user, review r2): waive the missing `claude plugin validate .` run (CLI not installed in this environment); it is owed to the person reviewing the PR before release. ROADMAP carries a "Written, not yet Shipped" partial note.
