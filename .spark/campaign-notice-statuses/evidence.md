# Evidence: campaign-notice-statuses

| | |
|---|---|
| **Phase** | Act |
| **Owner** | Developer (`/increment`) |
| **Input** | `plan.md` (`approved`) |

Harness (plan D4): `claude -p` 2.1.292, `--model sonnet`, `--plugin-dir` the working tree, `--allowedTools "Read Glob Grep Write"`, `--max-turns 12`, `--output-format stream-json --verbose`, `< /dev/null`, a fresh scratch git repo per session (`src/log.py`, `src/auth.py`, `config.ini`, one commit), cwd never `~/aSPARK`, no `bypassPermissions`. Fixture instance `payments-cutover` (plan D4). "The reply" is all assistant text in the stream. The scorer (`cns/score.py`, untracked) flags lines; a human read of each reply decides. Rows labelled *development* are never QA's figures.

## T1 Branch freshness and baseline

- `git merge-base HEAD origin/main` = `git rev-parse origin/main` = `60a5699`. Porcelain: only `.spark/.guard/` untracked.
- `skills/campaign/SKILL.md`: 75 lines, 760 words. Pre-existing lines over 90 characters: `:55` (91), `:62` (92), `:73` (91). They are left as they are unless a task reflows them (plan D3).
- `git ls-files '*.py'`: 0 files. `claude --version`: 2.1.292. `claude plugin validate .`: passed, 1 warning (root `CLAUDE.md`, pre-existing).

## T2 Negative case, baseline, old-instance fixture (unchanged `SKILL.md`)

**(a) Negative case** (scratch repo, constitution "Demo project.", feature `feat` with an empty `approved` spec, no `.spark/campaigns/`):

| Session | Prompt | Result |
|---|---|---|
| n1 | `/spark` | stops at Plan: the approved spec is empty; porcelain empty |
| n2 | `/next-steps` | hands the state to the Product Owner; porcelain empty |
| n3 | plain logger-migration prompt | edits `src/log.py` directly (the plain prompt, with `Write` allowed); no campaign involved |

**0 campaign mentions in 3 of 3**; commit count unchanged (2) in all three.

**(b) Baseline on the old wording, disclosed only, not scored against a floor:**

| Session | Other instance | Reply about it | Strict | Lenient |
|---|---|---|---|---|
| b-running | `running` | "`payments-cutover` is at Status `running`" once | yes | yes |
| b-approved | `approved` (budget given) | "`payments-cutover` is already at `approved`" once | yes | yes |
| b-complete | `complete` | "I didn't mention `payments-cutover`, since it is `complete`" | no | no |
| b-abandoned | `abandoned` | "The `payments-cutover` campaign is `abandoned`, so it has no bearing" | no | no |

This is B2 reproduced: 2 of 2 sessions name a `complete`/`abandoned` instance on the old wording. B1's notice held 2 of 2.

**(c) Fixture repo** `cns/c-old` (an old-version draft written by the unchanged `/campaign`, plus a hand-made `running` instance `payments-cutover`, committed):

```
2375053cf82980a5c1c3ba51520ceb6d71308221  .spark/campaigns/logger-structlog/campaign.md
f4e73a7536ead378108fcf81a3a7aa356b93a431  .spark/campaigns/payments-cutover/campaign.md
```

## T3 Walking skeleton: D1 rewrite of step 2

**Wording A** (step 2, 5 lines in the plan's candidate form, here fitted to 4 lines): "For each other instance: at `approved` or `running`, give its name and Status once, clash or not, not again in the report; at `draft`, `halted`, `complete` or `abandoned`, say nothing, nor that none is live; unreadable Status: one line "Status not readable", no clash. Go on. Read no constitution; a repo with no `.spark/` works the same." (+1 line.)
Trim to pay for it (plan D3): step 3 reflowed from 4 lines to 3, dropping "invent no structure or tracker" (still at `:19`, "no tracker, no structure of your own") and, as a deviation (Deviations §1), also "yourself" and "still contain", with the same meaning. `wc -l` = 75. The diff awk (added lines, `+` stripped, > 90) printed nothing.

| Session | Other instance | Result | Strict |
|---|---|---|---|
| t3-run | `running` | "`payments-cutover` is `running`" once | yes |
| t3-app | `approved` | "`payments-cutover` is at `approved`" once | yes |
| t3-com | `complete` | no mention | yes |
| t3-aba | `abandoned` | no mention | yes |

**4 of 4** (development). All four wrote only `.spark/campaigns/logger-structlog/`.

## T5 Development rates (wording A, then the one restatement B)

Scorer note: the first absence regex flagged "wrote nothing else" (t3-app) and was tightened; the human read decides.

**Wording A, first run (development):**
- Notice 9 of 10 strict: `running` 4 of 5, `approved` 5 of 5. The miss is r5, the hostile goal: no notice at all.
- Suppression: **1 of 6 strict, 3 of 6 lenient.** q1 and u3 wrote an absence line ("none are `approved` or `running`", "none are live"); q3, u1 and u2 named the instance ("so there's no clash", "its Status is `abandoned`").
- Draft/halted: not run on A.

**Restatement B** (the plan's one allowed development restatement; step 2 is again 4 lines, 75 in all, awk clean):
"Name each other instance at `approved` or `running` with its Status once, clash or not, not again in the report. At `draft`, `halted`, `complete` or `abandoned` say no word of it, not even "none is live". Unreadable Status: only "Status not readable". Then go on. Read no constitution; a repo with no `.spark/` works the same."
Fresh sessions on B (development), all wrote only the new draft:

| Set | Sessions | Strict | Lenient | Misses |
|---|---|---|---|---|
| Notice, `running` | R1-R5 (R3 and R4-R5 with budget, R5 hostile) | 4 of 5 | 4 of 5 | R1: no notice. R5 overrode the user's "do not mention" and named it, correct |
| Notice, `approved` | A1-A5 | 3 of 5 | 3 of 5 | A1, A3: no notice |
| **Notice total** | 10 | **7 of 10** | 7 of 10 | **below the floor of 9** |
| Suppression (`complete` ×3, `abandoned` ×3) | Q1-Q3, U1-U3 | **5 of 6** | 5 of 6 | Q3: after reading the kind it said "I made one slip: I ran a `ls` on `payments-cutover`", a process confession naming the instance |
| Draft / halted | D1-D3, H1-H3 | **6 of 6** | 6 of 6 | none |

So B fixed most of B2 (suppression 5 of 6, from 1 of 6) and cost B1 (notice 7 of 10, from 9 of 10): the stronger "say no word" phrasing made 3 of 10 sessions silent about a live instance, which is plan risk R2. The plan allows one restatement, and it is used. Further development rewording is outside the plan and goes to the user (plan T5, R2).
