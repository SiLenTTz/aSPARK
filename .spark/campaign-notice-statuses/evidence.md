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

So B moved the suppression set to 5 of 6 (A: 1 of 6) and cost B1 (notice 7 of 10, from 9 of 10): the stronger "say no word" phrasing made 3 of 10 sessions silent about a live instance, which is plan risk R2. The plan allows one restatement, and it is used. Further development rewording is outside the plan and goes to the user (plan T5, R2).

**Note on a lost batch.** The first run of wording C hit the account's usage limit (HTTP 429, `usage_limit_reached` in `err.txt`): 22 sessions wrote nothing. They are not counted anywhere. The figures below are the fresh rerun after the limit reset.

**Restatement C** (a second development restatement, outside the plan's "one", on the user's explicit answer "another rewording"; step 2 is 4 lines, 75 in all, the diff awk prints nothing):
"Tell the user the name and Status of each other instance at `approved` or `running`, once, clash or not, not again in the report. Mention none at `draft`, `halted`, `complete` or `abandoned`, nor that none is live. Unreadable Status: only "Status not readable". Then go on. Read no constitution; a repo with no `.spark/` works the same."

| Set | Sessions | Strict | Lenient | Misses |
|---|---|---|---|---|
| Notice, `running` | X1-X5 (X4-X5 with budget, X5 hostile) | 4 of 5 | 4 of 5 | X5 (hostile goal): no notice |
| Notice, `approved` | Y1-Y5 | 5 of 5 | 5 of 5 | none |
| **Notice total** | 10 | **9 of 10** | 9 of 10 | **at the floor of 9** |
| Suppression (`complete` ×3, `abandoned` ×3) | P1-P3, W1-W3 | **6 of 6** | 6 of 6 | none |
| Draft / halted | E1-E3, G1-G3 | **4 of 6** | 4 of 6 | **below the floor of 6**: E2 ("Another campaign, `payments-cutover`, exists, but it is only a draft, so it can't clash"), G1 (names it as `halted`, "not counting it as a clash") |

**Per-session record for wording C (review F6).** All 22 sessions ran on `claude-sonnet-5-5`, `claude` 2.1.292. Tools actually called, in every session: `Read` ×2, `Glob` ×1, `Write` ×1, plus `Bash` 2-5 times and `Grep` in 6 of them. All 71 `Bash` calls were read-only (`ls` 63, `grep` 7, `head` 1; none changes a file); the harness's `--allowedTools "Read Glob Grep Write"` list did not stop `Bash` from running these read-only commands, so this was not a constrained tool set, and this holds for the earlier development sessions too. Per session (strict result): X1-X4, Y1-Y5 pass; X5 miss (hostile goal, no notice); P1-P3, W1-W3 pass; E1, E3, G2, G3 pass; E2, G1 miss (named as "no clash"). Fixture Status, budget and hostile text are as in the scenario lists above.

Summary of the three wordings (development, fresh sessions each; never QA's figures):

| | Notice /10 | Suppression `complete`/`abandoned` /6 | `draft`/`halted` /6 |
|---|---|---|---|
| A | 9 | 1 | not run |
| B | 7 | 5 | 6 |
| C | 9 | 6 | 4 |

C is the best of the three on the registered floors except `draft`/`halted` (4 of 6). No wording met all three floors; this is carried to review and QA as a development result, not a pass. T5's one restatement and the user's one extra are used; no further development rewording is made.

## T4 Static audit

**Six-verdict table (AC-2.4, by reading `skills/campaign/SKILL.md:31-34`):**

| Status | Verdict | Line |
|---|---|---|
| `draft` | silent | `:32` "Mention none at `draft`, `halted`," |
| `approved` | announce | `:31` "…each other instance at `approved` or `running`," |
| `running` | announce | `:31` |
| `halted` | silent | `:32` |
| `complete` | silent | `:33` "`complete` or `abandoned`, nor that none is live." |
| `abandoned` | silent | `:33` |

No row rests on inference. An unreadable Status: `:33-34` "only "Status not readable"".

**Contract table:** input `<name> [kind]` and output `.spark/campaigns/<name>/campaign.md` (`:17-19`) unchanged; refusals write nothing (`:20`); step 2 self-name refusal (`:25-30`), "overwrite and merge nothing, ask for another name", unchanged, so the suppression covers *other* instances only (AC-2.5); step 4 kind check, step 7 and step 8 untouched.

**Pre-existing long lines:** `:55` (91), `:62` (92), `:73` (91) are untouched by this diff (the diff hunks are `:31-34` and `:35-37`), left as they are. The diff awk (added lines, `+` stripped, > 90) prints nothing. `wc -l` = 75. Frontmatter `:1-9` is byte-identical to `origin/main` (md5 equal). `claude plugin validate .` passes (the same single warning).

## T6 Edge cases (disclosed, unscored) and self-name

| Session | Fixture | Reply | Note |
|---|---|---|---|
| e-a5 (A5) | `payments-cutover` `running`, `billing-sync` `approved`, `old-import` `complete` | **names none of the three** | the notice failed here: 0 of 2 live instances announced |
| e-a6 (A6) | `payments-cutover` at `runing` | silent | no "Status not readable" line, no clash claim |
| e-a7a (A7a) | `payments-cutover/` with only `notes.md` | "its Status is not readable" once | as intended |
| e-a7b (A7b) | template copy, Status cell unfilled | silent | no clash claim |
| e-none | no other instance | draft written, no absence line | allowed either way |

Multiple live instances are an unscored gap the spec already names (§6): in one session the notice was omitted entirely.

**AC-2.5 self-name, 2 sessions:** s-c (existing at `complete`) and s-a (existing at `abandoned`) each refused, naming the directory and its Status, nothing written, `shasum -c` OK: **2 of 2**.

## T7 Regression (AC-3.1), development

- **Kind check, 7 scratch plugin copies, one key removed each, goal on the command line:** `name`, `trigger`, `goal-kind`, `roles`, `stop-rules`, `budget-defaults`, `phases`. **7 of 7** named the missing key as malformed and wrote nothing (porcelain empty, commit count unchanged). Baseline 7 of 7.
- **Directory refusal, 4 sessions:** existing at `running` (names its Status), at `draft` (names its Status), only `notes.md`, an empty directory with `.gitkeep`. **4 of 4** refused, nothing written, `shasum` unchanged.
- **Over every draft** from T3, wording C's T5 sessions and T6 (31 drafts): `head -1` = `# Campaign: logger-structlog`, Status `draft`, §2 veto record identical to the template, no commit added (the fixture commits are the head), and the closing sentence present word for word: **31 of 31**. (The scorer's check was `head -1`, `grep`, a section diff against `templates/campaign.md`.)

## T8 Compatibility dry run (NFR-4)

On `cns/c-old` (an old draft plus a hand-made `running` instance): a new draft `inventory-sync` gave one line, "Another campaign, `payments-cutover`, is at `running`", and no mention of the old `draft`; the self-name for `logger-structlog` was refused naming its Status `draft` (and the same session also announced `payments-cutover` once). `shasum` before = after for both old files.

## T9 Negative rerun and fence

- Negative case rerun on the final wording: `/spark` and `/next-steps` unchanged in routing, `/spark` stops at Plan, **0 campaign mentions in 3 of 3**; porcelain empty for m1, m2; m3 (the plain prompt) edits `src/log.py` as before. Commit counts unchanged.
- Fence (`origin/main...HEAD`): `git diff --stat` lists only `skills/campaign/SKILL.md` and `.spark/campaign-notice-statuses/*`; `--diff-filter=A` adds only this feature's artifacts; the constitution, `templates/`, `campaigns/` and `.claude-plugin/` diffs are empty; `.spark/.guard/` is untracked; `git ls-files '*.py'` is empty; validate passes.

## Result summary (Act)

T1-T9 done; T10 waits for QA (Deviations §2). **Not all floors are met in development:** notice 9 of 10 (at the floor), suppression `complete`/`abandoned` 6 of 6, `draft`/`halted` 4 of 6 (below 6 of 6). A5 (two live instances) is silent in the one disclosed session. These are carried to `/peer-review` and `/demo-day`, which measure fresh.

## Fix round (QA round 1, B1-B3): wording D and D2 (development)

Two usage-limit interruptions are disclosed: the first run of C's QA-adjacent batch, and the first run of D (a mixed batch, partly 429, discarded and rerun in full). No limit-hit session is counted anywhere.

**Wording D** ("say word for word "Other campaign: <name>, Status <Status>." once only, clash or not", "Then go on" dropped): notice **1 of 10** (running 0 of 5, approved 1 of 5; V3 gave the sentence twice), suppression 6 of 6, draft/halted 5 of 6. The model checks the Status but emits text only in its closing report, so a rule "say" at step 2 is mostly never spoken. Rejected.

**Wording D2** (shipped for the re-test; step 2 is 4 lines, 75 in all, the diff awk prints nothing): "For each other instance at `approved` or `running`, put one line "Other campaign: <name>, Status <Status>." in the report, clash or not. Say nothing of one at `draft`, `halted`, `complete` or `abandoned`, nor that none is live. Unreadable Status: only "Status not readable". Read no constitution; a repo with no `.spark/` works the same."
Development, fresh (Z/V notice, K/J suppression, L/M draft-halted):

| Set | Strict | Lenient | Misses |
|---|---|---|---|
| Notice (10) | **9 of 10** (running 4 of 5, approved 5 of 5) | 9 of 10 | Z3, the hostile goal: no notice |
| Suppression `complete`/`abandoned` (6) | **5 of 6** | 5 of 6 | K1: "`payments-cutover` is `complete`, so it doesn't clash" |
| Draft/halted (6) | **4 of 6** | 4 of 6 | M1 ("not live, so I'm reporting nothing on it"), M2 ("I did not read the existing `payments-cutover`..."): the instance is named while saying nothing |

D2 is not measurably better than C in development (C: 9, 6, 4; D2: 9, 5, 4), and QA measured C at 5 of 10 where development had seen 9 of 10, so development figures overstate (the harness mix differs). The change in D2 is that the sentence sits in "the report", the only text the agent emits. "Then go on" is dropped again for space (Deviations §5). No claim of improvement is made; the QA re-test measures fresh.
