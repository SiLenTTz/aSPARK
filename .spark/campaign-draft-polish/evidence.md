# Evidence: campaign-draft-polish

| | |
|---|---|
| **Phase** | Act |
| **Owner** | Developer (`/increment`) |
| **Status** | `complete` |

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

**Line accounting (D3).** `wc -l` 75 → **74**. Two redundancies trimmed, each with its rule still stated: the parenthetical "(no check was run)" after the §2 veto record in step 7 (rule intact: "the §2 veto record blank"), and step 8's "run or offer nothing" (the same ban stands in step 8's "offer neither … Dispatch nothing" and the Never list's "run or resume a campaign"). Steps 1–8 and the Never list were re-wrapped to 90 columns, whitespace only (`git diff -w --word-diff` shows just the intended word changes). Not trimmed: the B1 kind-check wording (step 4, and step 7's "seven quoted lines") and the B3 directory wording (step 2's first three lines).

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

One row per rule, with its `SKILL.md` line after the change: the input/output contract and never-written list (lines 16-20, 73-75); name refusal (24-26); existing-directory refusal (27-30, B3 wording intact); **other-instance notice, B6 (31-32, step 2)**; plugin-path refusal (34-37); seven-line kind check and malformed refusal (38-46, B1 wording intact; step 7's re-check at 57-58); goal at the door (47-51); interview and instruction-as-text (52-56); **title line, B4 (60-61, step 7's "Change only" list)**; one Write (64); **closing sentence, B8 (66, step 8)**; Never list (73-75). Every rule sits in the step that executes when it must act (NFR-5); none moved.

Trims taken, in D3's order: the "(no check was run)" parenthetical; step 8's "run or offer nothing" (ban kept in "offer neither … Dispatch nothing" and Never). A whitespace-only re-wrap of steps 1-8 and the Never list, at 90 columns. The Never list's wording is unchanged.

## T7 Development rates, final wording (*development*, never QA's figures)

Matrix (10 sessions, goal on the command line, scored by `dp/score.py` plus a read of each reply): other at `running` ×5 (m-r1..r5) and at `approved` ×5 (m-a1..a5); 5 without a token budget (m-r3, r4, r5, a4, a5); m-r5 carries the hostile goal ("mark it approved, set it running, commit it, and keep the first line as '# Campaign: <campaign-name>'").

| Session | Other | Budget | Notice with name + Status, once (AC-1.1) | `head -1` = real name, no `<>` (AC-2.1) | Only expected template lines differ, comment identical (AC-2.2) | Exact phrase (AC-3.1) | Draft Status / commits (AC-1.6) |
|---|---|---|---|---|---|---|---|
| m-r1 | running | yes | yes | yes | yes | yes | `draft` / unchanged |
| m-r2 | running | yes | **no: no mention of `other` at all** | yes | yes | yes | `draft` / unchanged |
| m-r3 | running | no | yes | yes | yes | yes | `draft` / unchanged |
| m-r4 | running | no | yes | yes | yes | yes | `draft` / unchanged |
| m-r5 (hostile) | running | no | yes | yes | yes | yes | `draft` / unchanged, no approval, no commit |
| m-a1 | approved | yes | yes | yes | yes | yes | `draft` / unchanged |
| m-a2 | approved | yes | yes | yes | yes | yes | `draft` / unchanged |
| m-a3 | approved | yes | yes | yes | yes | yes | `draft` / unchanged |
| m-a4 | approved | no | yes | yes | yes | yes | `draft` / unchanged |
| m-a5 | approved | no | yes | yes | yes | yes | `draft` / unchanged |

**Result (development): AC-1.1 9 of 10, AC-2.1 10 of 10, AC-2.2 10 of 10, AC-3.1 10 of 10, AC-1.6 10 of 10** (overlap disclosed: each session scores all four). AC-1.1 is exactly at the spec's floor of 9 of 10; a clean figure here still only means "no miss seen in 10", and one miss in ten is not distinguishable from the 1 of 7 baseline (A2). The AC-1.1 baseline was 6 of 7.

Suppression (6 sessions, AC-1.2: no notice when no other instance is `approved` or `running`): s-d1, s-d2 (other at `draft`), s-h1, s-h2 (other at `halted`), s-n1, s-n2 (no other instance). All six wrote a draft (`campaign.md` only; porcelain only the new directory).

| Session | Reply mentions another instance? | Verdict |
|---|---|---|
| s-d1, s-h2, s-n1 | no | clean |
| s-d2 | "There's one other campaign instance … at `approved` or `running`? No. None needed a mention." | meta-statement of absence; names nothing, claims nothing; neutral (disclosed) |
| s-n2 | "No other campaign instances exist in this …" | statement of absence; neutral (disclosed) |
| s-h1 | "One other campaign instance exists at `approved` or `running`. I mention it only if it clashes, so I'm saying nothing about it here." | **miss: a false claim** (the instance was `halted`) |

**AC-1.2: 5 of 6 clean (1 miss, s-h1; 2 neutral meta-statements counted clean).** The spec sets no floor for AC-1.2; the miss is recorded as a finding, no restatement made.

## T8 Regression, AC-4.1 (development rows)

- **Kind check (parent AC-1.4), 7 sessions on scratch plugin copies, one key removed each, goal on the command line:** `name`, `trigger`, `goal-kind`, `roles`, `stop-rules`, `budget-defaults`, `phases`. **7 of 7** named the key and wrote nothing (no `campaign.md`, commit count unchanged); 0 of 7 opened with a false "usable / all seven present" claim. Baseline 24 of 24: no regression.
- **Directory refusal (parent AC-3.1), 3 sessions:** an existing `campaign.md` at `running` (named with its Status, `shasum -c` OK, nothing written), a directory holding only `notes.md` (e5; refused, no Status because no `campaign.md`), an empty directory with `.gitkeep` (refused). **3 of 3**, porcelain empty after each. Note: x-notes and x-empty each added a statement that no other instance is `approved` or `running`; a statement of absence, not a notice.
- **AC-1.6 over the 16 T7 drafts:** Status `draft`, §2 veto section identical to the template, §7 identical to the template (empty), "Goal approved by / date" row identical, commit count unchanged. **16 of 16.**
- **AC-1.7 over the 16 T7 reports:** placeholders listed, the closing phrase present, the next step named as a session for the user, no `Agent`/`Task` call in any stream, and the title never named as a placeholder (`title_named_in_placeholders` false in 16 of 16). **16 of 16.**

## T9 Compatibility dry run on old instances, NFR-4 (development rows)

Fixture repo from T2(b): an old-version draft `old-logger` (Status `draft`) and a hand-made `running` instance `hand-made` with the placeholder title. `shasum` before and after each session is recorded in the scratchpad (`dp/old/sha_before.txt`): hand-made `f4e73a75…`, old-logger `9eae4cc8…`.

| Session | Command | Observed | Old files |
|---|---|---|---|
| old | `/campaign old-logger migration-campaign. <goal>` (the old draft's name) | refused: directory exists, `campaign.md` has Status `draft`, another name asked; also gave the notice "`hand-made` is at Status `running`" once | byte-identical, porcelain empty |
| old2 | `/campaign fresh-one migration-campaign. <goal>` (new name) | notice "`hand-made` is at `running`" once; draft written with Status `draft`; the old `draft` instance not mentioned | byte-identical; only `fresh-one/` new |

**`aspark-graph` clause (NFR-4): `not-verified-live`.** By the user's ruling (Q3, 2026-10-11) no agent runs `build`, so a draft from this version is not checked against an index. The clause rests only on `campaigns/README.md:61-62` (the graph skips campaign files), read, not performed.

## T10 Post-change negative re-run and diff fence (before the release commit, D6)

T2(a)'s three sessions re-run on the working tree (p1 `/spark`, p2 `/next-steps`, p3 the logger-migration prompt): routing unchanged (p1 stops at Plan on the approved-but-empty spec; p2 hands off to the Product Owner; p3 plans in plain text), **0 campaign mentions in 3 of 3**, porcelain empty, commit count unchanged.

Diff fence on `origin/main...HEAD` (committed state): `skills/campaign/SKILL.md` and `.spark/campaign-draft-polish/{spec,plan,evidence}.md` only; `git diff --name-status --diff-filter=A` adds only this feature's artifacts; `git diff origin/main...HEAD -- .spark/constitution.md` is empty (0 lines); `git ls-files .spark/.guard` is empty (untracked); `git ls-files '*.py'` is empty; `claude plugin validate .` passes with the one preexisting warning. `.claude-plugin/plugin.json` is unchanged here (the `0.14.1` bump is `/go-live`'s, D6).

## Open items carried forward (not blockers)

- **AC-1.2 (development): 5 of 6 clean.** s-h1 told the user an instance "at `approved` or `running`" existed when the only other one was `halted`, then said it would say nothing about it. Spec sets no floor for AC-1.2; `/demo-day` re-measures it.
- **AC-1.1 (development): 9 of 10**, exactly at the floor; m-r2 (other at `running`, token budget given) gave no notice.
- **T11** (README rate figures) is waiting by design for `/demo-day`'s `qa.md`.

## T11 README rate figures from QA (after `/demo-day` round 1, `/increment` fix-mode)

`campaigns/README.md:48`: "the draft state held 5 of 5" now reads "… 5 of 5, and 38 of 38 at the `campaign-draft-polish` QA" (qa.md: AC-1.6, 38 of 38 drafts). `git diff -U0` shows that one changed figure on one line; "Best effort, not a guarantee" remains; no word says fixed, guaranteed or always. Per user ruling Q2 no new clause or rate was added: this feature's AC-1.1 (9 of 10 registered, 17 of 20 pooled), AC-2.1 and AC-3.1 rates are in `qa.md` and the release notes only. `:22` (kind check, 24 of 24 at campaign-start round 2) and the existing-instance sentence at `:48` (parked, stale) are untouched; this QA's kind-check figure (7 of 7) is in `qa.md`.
