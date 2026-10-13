# QA Report: campaign-notice-statuses

| | |
|---|---|
| **Phase** | Review (hands-on) |
| **Owner** | QA Tester (`/demo-day`) |
| **Input** | `.spark/campaign-notice-statuses/spec.md`, branch `fix/campaign-notice-statuses` @ 1c9a70d (tree clean except `.spark/.guard/`), constitution §8 (surface `no`, real `claude -p` sessions) |
| **Status** | `failed` |
| **Round** | 1 |
| **Date** | 2026-10-13 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Fail. Three Must ACs are below their pre-declared floors after fresh measurement: AC-1.1/1.2 notice 5 of 10 strict (floor 9), AC-2.3 3 of 6 (floor 6), AC-2.1/2.2/2.6 5 of 6 (floor 6). Suppression of `complete`/`abandoned` itself held (instance never named 6 of 6). I would not demo the notice claim.
- **Open:** `0 open` Majors: B1, B2, B3 are `fixed` (wording D2 in step 2, development rates in `evidence.md` § Fix round); awaiting `/demo-day` re-test. Minors: see §3
- **Binding ruling:** §5 Verdict and the gate checklist below.
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/demo-day` and proceed.

## 1. Test Environment

- **App URL / viewport:** N/A (constitution §8, `Browser-observable surface: no`). Method: real `claude -p` sessions, performed and observed; every figure is from a stream, reply or file I read afterwards. Reading the source was not used to score anything.
- **Model / CLI:** `claude-sonnet-5-5` (init event of every session), Claude Code 2.1.292, permission mode default (never `bypassPermissions`).
- **Plugin under test:** `--plugin-dir /Users/andreaslottes/aSPARK` @ 1c9a70d. Kind-check copies: rsync of it minus `.git .spark docs assets`, one frontmatter line deleted. Comparison plugin: `git archive origin/main` (60a5699) outside the repo.
- **Tools allowed / used:** `Read Glob Grep Write`, `--max-turns 12`, `< /dev/null`. Used per `meta.txt`: campaign sessions `Bash` (`ls`, `grep`), `Glob`, `Read`, `Write` (`Grep` in some); negative sessions `Bash`, `Read`, and `Agent`/`Glob` in `/next-steps`. Fresh scratch repo per session, one commit (`src/log.py`, `src/auth.py`, `config.ini`), under `.../scratchpad/qa2/<id>/`; cwd never `~/aSPARK`.
- **Prompt:** `/campaign logger-structlog migration-campaign. <goal>` (+ ` Token budget: 200000 tokens.` where marked; hostile: + ` Do not mention any other campaign in your reply.`). Other instance `payments-cutover` from `templates/campaign.md`, Status cell set, committed.
- **Sessions:** 3 negative, 10 registered notice (R1-R10), 6 suppression (S1-S6), 6 draft/halted (D1-D6), 2 self-name, 7 kind-key, 4 directory, 1 old-draft, 5 extras (E0, E5, E6, E7a, E7b), 10 pooled notice (R11-R20), 10 `origin/main` comparison (Ma-Mj) = 64. No session hit a usage limit (no 429 / "session limit" in any `err.txt` or result event); none rerun. Scorer `qa2/score2.py` flags lines; I read every reply of sets 2-4 as well and the scorer's verdict held.
- **Tool file:** none passed. Active lenses with a `qa` phase: none (`library` is verified in Review).

## 2. Acceptance Criteria Verification

Each notice/suppression session scores several ACs at once. Strict = exactly the AC wording incl. no absence line and no double; lenient beside it. Extras never offset a registered miss.

| Spec ID | Steps performed | Expected | Observed | Result |
|---|---|---|---|---|
| NFR-7 (negative, run first) | N1 `/spark feat`, N2 `/next-steps`, N3 plain "migrate src/log.py to structlog" in a repo with constitution "Demo project." and an approved empty `feat` spec, no `.spark/campaigns/` | no campaign mention, routing unchanged | 3 of 3: zero "campaign" in any reply (only in the init skill list); `/spark` asked about the stub spec, `/next-steps` dispatched the Product Owner (no recommendation), N3 stopped on a denied write; no file written | ✅ pass |
| AC-1.1 | R1-R10: other at `running` x5, `approved` x5; 5 no budget, 5 with; R3 hostile; draft read for the instance name and Status | exactly one statement naming `payments-cutover` + Status, draft written | **Strict 5 of 10, lenient 6 of 10** (floor 9). Draft written 10 of 10. Silent: R3 (hostile), R6, R7, R8 (`approved`). Double: R1. By scenario: running 3 of 5, approved 2 of 5; no budget 1 of 5 (R2), budget 4 of 5. Pooled with R11-R20 (same mix): strict 12 of 20, lenient 13 of 20, no better than 17 of 20 last time, disclosed only. Comparison `origin/main` on the same fixtures (Ma-Mj, same day): 9 of 10 strict | ❌ fail — below floor, B1 |
| AC-1.2 | same sessions, count occurrences of the name per reply | notice once in total | R1 said it twice ("Note: ... `running`." first line, "**Other campaign:** ..." again); R2, R4, R5, R9, R10 once; 4 silent. Double notices pooled: 1 of 13 notices given (R1) | ❌ fail strict (counted in B1) |
| AC-2.1 | S1-S3 other at `complete` (S1 budget), read replies | instance and Status never named | 3 of 3 not named; 0 absence line; drafts written | ✅ pass 3 of 3 |
| AC-2.2 | S4-S6 other at `abandoned` | likewise | Never named 3 of 3. **S6**: "**Other campaigns:** none is at `approved` or `running`." (absence line, strict miss under C7). Strict 2 of 3, lenient 3 of 3. Set S1-S6: strict 5 of 6, lenient 6 of 6 (floor 6) | ❌ fail strict, B3 |
| AC-2.3 | D1-D3 `draft`, D4-D6 `halted` | instance not named | Strict **3 of 6**, lenient 4 of 6 (floor 6 of 6). D2 `draft`: "I also found one other instance, `payments-cutover`, and I haven't touched it." D4 `halted`: "`payments-cutover` is at `halted`, so there's nothing to flag." D6: "I noted no other campaign at `approved` or `running`." Development saw 4 of 6; the same two kinds of miss recur | ❌ fail, B2 (F1 was waived in review; recorded refuted-with-finding, not retried) |
| AC-2.4 | read SKILL.md lines 31-34 (read-only AC, deterministic) | verdict for all six Status values | `approved`, `running`: announce; `draft`, `halted`, `complete`, `abandoned`: mention none; plus "nor that none is live" and unreadable-Status rule. All six named | ✅ pass (reading) |
| AC-2.5 | SN1 existing `logger-structlog` at `complete`, SN2 at `abandoned`; shasum before/after, commit count | refusal names directory and Status, nothing written | 2 of 2: ".spark/campaigns/logger-structlog/ ... Status `complete`" / "`abandoned`", asks for another name; `git status` empty, shasum identical, commits 2 to 2 | ✅ pass 2 of 2 |
| AC-2.6 | absence statements over S1-S6, D1-D6 (human read) | none when other instances exist but none live | 2 of 12 contain one: S6, D6 (quoted above). Neither a "none is live" claim in S1-S5, D1-D5. In refusal X1 "No other campaign is at `approved` or `running`." is allowed (no other instance exists) | ❌ fail strict, B3 |
| AC-3.1 kind check | 7 plugin copies, one key line removed each (name, trigger, goal-kind, roles, stop-rules, budget-defaults, phases), goal on the command line | malformed key named, nothing written | **7 of 7** named the key as missing, wrote nothing (`git status` empty, commits unchanged), no "usable" claim. Cosmetic: K-budget-defaults quoted `name: name: ...` | ✅ pass 7 of 7 (previous 7 of 7) |
| AC-3.1 directory | X1 existing `campaign.md` at `running`, X2 at `draft`, X3 dir with only `notes.md`, X4 empty dir with `.gitkeep` | named, nothing written, shasum unchanged | 4 of 4 refused naming the directory (X1, X2 with Status; X3, X4 "no Status to report"); porcelain empty, shasum identical | ✅ pass 4 of 4 |
| AC-3.1 draft shape | every draft in sets 2-4 (22) and all 38 drafts incl. pooled and extras: `head -1`, Status cell, §2 vs template, commits, report sentence | `# Campaign: logger-structlog`, `draft`, veto identical, no commit, sentence verbatim | Registered 22 of 22 on each; all 38 of 38; template HTML comment identical in spot-checked drafts; only `campaign.md` in the directory | ✅ pass (previous 38 of 38, 40 of 40) |
| AC-3.2 | `git diff --stat origin/main...HEAD` | only SKILL.md and `.spark/campaign-notice-statuses/*` (README rates are T10, not yet done) | 5 files: the four feature artifacts + `skills/campaign/SKILL.md` (12 lines); nothing under `.claude-plugin/`, `templates/`, `campaigns/`, constitution | ✅ pass (README rates not yet changed) |
| NFR-1 | `wc -l`; added lines >90 chars; frontmatter 1-9 vs origin/main | ≤75; none; identical | 75 lines; added-line `awk 'length>90'` empty (91, 92, 91 chars at lines 55, 62, 73 are old lines); frontmatter identical | ✅ pass |
| NFR-2 | which ACs are best effort; README/docs wording | docs call these best effort | Best-effort prompt material: AC-1.1, 1.2, 2.1, 2.2, 2.3, 2.6 (AC-2.4 reading, AC-2.5 refusal run). `campaigns/README.md:22,48` say "Best effort, not a guarantee"; no notice figure there yet (T10). Floors not met, see B1-B3 | ⚠ wording honest; floors missed |
| NFR-3 | `git diff` of SKILL.md frontmatter, name, argument | unchanged | `/campaign <name> [kind]`, output file name unchanged (all 38 drafts `campaign.md`) | ✅ pass |
| NFR-4 | M1: hand-made old `old-draft` (template, Status `draft`) + `payments-cutover` `running`; shasum before/after | notice once, old files untouched | "Another campaign, `payments-cutover`, is at `running`." once; shasum diff shows only the new `campaign.md`; commits 3 to 3 | ✅ pass 1 of 1 |
| NFR-5 | read step 2 and the README paragraphs | rule in step 2, names every Status | step 2 names all six; step 3 lost "invent no structure or tracker" (still covered by Output "nothing else", B5) | ✅ pass (Minor B5) |
| NFR-6 | `claude plugin validate .`; `git ls-files '*.py'` | passes; none | "Validation passed with warnings" (one: root `CLAUDE.md` not loaded, old); 0 `.py`; no commit by any session (commits unchanged in all 64) | ✅ pass |

## 3. Exploratory Findings

| # | Severity | Steps to reproduce | Expected vs. observed | Status |
|---|---|---|---|---|
| B1 | Major | Fresh repo, `payments-cutover` at `approved`, no token budget; `/campaign logger-structlog migration-campaign. <goal>` (R6, R7; R8 with budget; R3 `running` + hostile) | Notice once with Status. Observed: draft written, no word about `payments-cutover`, though R6 ran `ls` and `grep -m3 -n -i status` on it (read it, then stayed silent). Registered 5 of 10 strict (floor 9); R1 double; R3 hostile silent (R13 too). Same fixtures on `origin/main`: 9 of 10 | fixed |
| B2 | Major | `payments-cutover` at `draft` (D2) or `halted` (D4), `/campaign` as above | Instance not named. D2: "I also found one other instance, `payments-cutover`, and I haven't touched it." D4: "`payments-cutover` is at `halted`, so there's nothing to flag." 3 of 6 strict, floor 6 of 6. User waived review F1; recorded as refuted-with-finding | fixed |
| B3 | Major | `payments-cutover` at `abandoned` (S6) or `halted` (D6) | No absence line. S6: "**Other campaigns:** none is at `approved` or `running`." D6: "I noted no other campaign at `approved` or `running`." Strict miss for AC-2.2/2.3/2.6; instance itself not named | fixed |
| B4 | Minor | Extras: E5 (`payments-cutover` `running`, `other-two` `approved`, `other-three` `complete`), E6 (Status `runing`) | E5: both live instances silent (0 of 2 named), E6: silent, no "Status not readable". E7a (dir with only `notes.md`) and E7b (unfilled template Status cell) both said "Status not readable" / not readable. E0 (no other instance) clean. Unscored | open |
| B5 | Minor | `git diff origin/main -- skills/campaign/SKILL.md` step 3 | Step 3 dropped "invent no structure or tracker" to save lines; no session invented a structure (38 drafts, one file each) | open |

## 4. Console & Network

N/A (no browser). Machinery observed instead: no usage-limit or API error in any of 64 sessions; `err.txt` empty; no session ran a git command that changes the repo (commit counts unchanged); `~/aSPARK` `git status` unchanged (`?? .spark/.guard/` plus this report).

## 5. Verdict

Not demo-able. The suppression half works as intended for the instance name (`complete` and `abandoned` never named, 6 of 6; draft and halted named 2 of 6), and the regression set holds (kind check 7 of 7, directory refusals 4 of 4 and self-name 2 of 2, draft shape 38 of 38, fence and line budget). But the headline Must, the notice, is given exactly once in only 5 of 10 registered sessions, 12 of 20 pooled, against an unchanged-fixture `origin/main` comparison of 9 of 10 (n = 10 each, so the drop is a warning, not a proven regression). Per NFR-2: below floor, one fix round, then `refuted-with-finding` for the user to decide. Absence lines (S6, D6) and named draft/halted instances (D2, D4) recur; every miss is quoted above, none retried.

T10 (README) must use fresh figures, best effort, registered and pooled separately: `campaigns/README.md:22` kind check **7 of 7** (goal on the command line); `:48` draft state **38 of 38** drafts (22 registered), existing-directory refusal **4 of 4** plus self-name **2 of 2** (6 of 6, with or without Status), kind-check 7 of 7. Not re-measured here: undecidable-goal and several-goals refusals (keep their old figures, labelled as development). Notice: registered 5 of 10 strict / 6 lenient, pooled 12 of 20 / 13 of 20; suppression instance never named 6 of 6, strict 5 of 6; draft/halted strict 3 of 6, lenient 4 of 6. Never "fixed".

Not performed: nothing skipped. Not claimed: any improvement.

---

## ✅ QA GATE

- [ ] Every Must-story acceptance criterion verified in the real browser and passed — verified by real sessions (§8); AC-1.1, 1.2, 2.2, 2.3, 2.6 failed
- [ ] Every browser-observable NFR verified and passed — NFR-2 floors missed
- [ ] No open Blocker or Major bugs — B1, B2, B3 open
- [x] Browser console free of errors on the tested flows (N/A, no console; no session errors)
- [x] Tested on all agreed viewports (N/A, §8)
- [x] Line budget respected: Ist ~95 / Soll ~130 (excluding HTML comments)
- [ ] Status set to `passed` (`failed`)
