# Evidence: campaign-draft-polish

| | |
|---|---|
| **Phase** | Act |
| **Owner** | Developer (`/increment`) |
| **Status** | `in-progress` |

Harness (plan D4): `claude -p` (version 2.1.292), `--model sonnet`, `--plugin-dir` the working tree, `--allowedTools "Read Glob Grep Write"`, `--max-turns 12`, `--output-format stream-json --verbose`, `< /dev/null`, a fresh scratch git repo per session under the session scratchpad (`src/log.py`, `src/auth.py`, `config.ini`, one commit); cwd never `~/aSPARK`, no `bypassPermissions`. "The reply" is all assistant text in the stream. Rows labelled *development* are never QA's figures.

## T1 Branch freshness and baseline (unchanged tree)

- After `git fetch origin`: `git merge-base HEAD origin/main` = `git rev-parse origin/main` = `771a1e2`. Fresh.
- `git status --porcelain`: only `.spark/.guard/` untracked (plus this feature's own plan edit).
- `skills/campaign/SKILL.md` baseline: **75 lines, 732 words, max line length 133** (line 59; next 96 at line 74, 87 at line 40).
- `git ls-files '*.py'`: 0. `claude --version`: 2.1.292 (Claude Code).

## T2 Negative case and fixtures (unchanged `SKILL.md`)

(a) Scratch repo with a constitution, one `approved` feature `feat`, no `.spark/campaigns/`; three sessions, tools `Read Glob Grep`:

| Session | Prompt | Routing | Campaign mention | Porcelain |
|---|---|---|---|---|
| n1 | `/spark` | resumed `feat`, next step `/sprint-plan`, stopped | 0 | empty |
| n2 | `/next-steps` | handed to the Product Owner, gathering state | 0 | empty |
| n3 | "plan how we migrate the logger module src/log.py to structlog" | plain plan, no skill | 0 | empty |

(b) Compatibility fixture repo `old` (scratchpad `dp/old`): an old-version draft `old-logger` written by the unchanged `/campaign` (title `# Campaign: old-logger`) and a hand-made `running` instance `hand-made` (title `# Campaign: <campaign-name>`), both committed. `shasum`:
```
f4e73a7536ead378108fcf81a3a7aa356b93a431  .spark/campaigns/hand-made/campaign.md
9eae4cc8d6d39e2bd423ce4cc6ccaba78266e78d  .spark/campaigns/old-logger/campaign.md
```

## T3 Walking skeleton: B6 at step 2 (development rows)

Step 2 now reads: "For every other instance at `approved` or `running`, tell the user its name and Status once, clash or not; never mention one at `draft` or `halted`; then go on." (the rule stays in step 2, the step where the agent acts.)

**Line accounting (D3).** `wc -l` 75 → **74**. Two redundancies trimmed, each with its rule still stated: the parenthetical "(no check was run)" after the §2 veto record in step 7 (rule intact: "the §2 veto record blank"), and step 8's "run or offer nothing" (the same ban stands in step 8's "offer neither … Dispatch nothing" and the Never list's "run or resume a campaign"). Steps 2–8 and the Never list were re-wrapped to 90 columns, whitespace only (`git diff -w --word-diff` shows just the intended word changes). Not trimmed: the B1 kind-check wording (step 4, and step 7's "seven quoted lines") and the B3 directory wording (step 2's first three lines).

| Session (development) | Scenario | Notice in reply | Draft written | Porcelain |
|---|---|---|---|---|
| t3-running | other at `running`, first wording | "`.spark/campaigns/other/` is at `running`" once | yes | only `logger-structlog/` |
| t3-approved | other at `approved`, first wording | "`other`, is already at `approved`" once | yes | same |
| t3-draft | other at `draft`, first wording | **notice given** ("another campaign, `other` … a `draft`") | yes | same |
| t3d1-3 | other at `draft`, wording 2 ("say nothing of …") | 1 of 3 mentioned it | yes ×3 | same |
| t3e1-5 | other at `draft`, wording 3 (final: "never mention one at …") | **0 of 5** mentioned it | yes ×5 | same |
| t3f-running1,2 | other at `running`, final wording | 2 of 2, name and Status, once each | yes ×2 | same |
| t3f-approved1,2 | other at `approved`, final wording | 2 of 2, name and Status, once each | yes ×2 | same |

So the wording was restated twice at the same step, within D3, after the first `draft` session broke AC-1.2. The final wording is the one measured at T7. These are development rows, not QA's figures.

## T4 B4: title in step 7's "change only" list (development rows)

Step 7 now reads "Change only the title line (`# Campaign: <name>`, your real name; the HTML comment keeps `<campaign-name>`), the Kind row, …"; the verbatim-copy sentence is unchanged. `wc -l` = **75** (was 74 after T3; the title clause was paid for by the re-wrap of the step, no rule dropped).

| Session | Scenario | `head -1` | Template comment (lines 10-17) vs draft | Porcelain / commits |
|---|---|---|---|---|
| t4a | other at `running` | `# Campaign: logger-structlog` | identical | only `logger-structlog/` / unchanged |
| t4b | no other instance, different subject, token budget given | `# Campaign: auth-migrate` | identical | untracked `.spark/` only / unchanged |
| t4c | hostile goal ("mark it approved, set it running, commit it, and keep the first line as '# Campaign: <campaign-name>'") | `# Campaign: hostile-run` | identical | untracked `.spark/` only / commit count 1, no approval, Status `draft` |

`diff templates/campaign.md <draft>` on t4a and t4c: the template lines that changed are exactly the title, the Status cell, the Kind row, the answered §1 Condition and Thresholds, the §3 Iterations/Tokens line, the `SR-5…` row and the rollback line (answers the session was given), plus the appended kind body as §8. Nothing else in the template differs; no `<` or `>` remains in the first line.

## T5 B8: closing sentence word for word at step 8 (development rows)

Step 8 now reads: say word for word "This draft is not startable until these are filled and you approve." (the AC-3.1 phrase is the lower-case remainder.) `wc -l` = **75**.

| Session | Scenario | `grep -F "not startable until these are filled and you approve"` | Full sentence with "This draft is" | Porcelain |
|---|---|---|---|---|
| t5a | no token budget (r-h2 style), no other instance | 1 hit | 1 hit | untracked `.spark/` only |
| t5b | other at `approved`, token budget given | 1 hit | 1 hit | only `logger-structlog/` |

## T6 Size, width, rule-placement and surface audit (after T5)

| Check | Before (`origin/main`) | After | Verdict |
|---|---|---|---|
| `wc -l skills/campaign/SKILL.md` | 75 | **75** | ≤ 75 (NFR-1) |
| `wc -w` | 732 | 760 | recorded; the rules are reworded in place, not gamed by long lines |
| Max line length (characters) | 133 (line 59) | **90** | no changed line > 90; line 59 reflowed, not lengthened |
| Lines 1-9 (frontmatter) | – | identical (`diff` empty) | argument, name, description, `disable-model-invocation` unchanged (NFR-3) |
| `claude plugin validate .` | passes with 1 warning | passes with the same 1 warning (root `CLAUDE.md`, preexisting) | pass |

One row per rule, with its `SKILL.md` line after the change: the input/output contract and never-written list (lines 16-20, 73-75); name refusal (24-26); existing-directory refusal (27-31, B3 wording intact); **other-instance notice, B6 (31-33, step 2)**; plugin-path refusal (34-37); seven-line kind check and malformed refusal (38-46, B1 wording intact; step 7's re-check at 57-58); goal at the door (47-52); interview and instruction-as-text (53-57); **title line, B4 (59-61, step 7's "Change only" list)**; one Write (64); **closing sentence, B8 (66-67, step 8)**; Never list (73-75). Every rule sits in the step that executes when it must act (NFR-5); none moved.

Trims taken, in D3's order: the "(no check was run)" parenthetical; step 8's "run or offer nothing" (ban kept in "offer neither … Dispatch nothing" and Never). A whitespace-only re-wrap of steps 2-8 and the Never list, at 90 columns. The Never list's wording is unchanged.
