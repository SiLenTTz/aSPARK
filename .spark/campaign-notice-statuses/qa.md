# QA Report: campaign-notice-statuses

| | |
|---|---|
| **Phase** | Review (hands-on) |
| **Owner** | QA Tester (`/demo-day`) |
| **Input** | `.spark/campaign-notice-statuses/spec.md`, branch `fix/campaign-notice-statuses` @ 65eca05 (tree clean except `.spark/.guard/`), constitution §8 (surface `no`, real `claude -p` sessions) |
| **Status** | `passed` |
| **Round** | 2 |
| **Date** | 2026-10-14 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Fail, narrower than round 1. The notice now meets its floor (AC-1.1/1.2 strict 9 of 10, was 5 of 10; pooled 17 of 20, was 12 of 20). Still below floor, unchanged from round 1: AC-2.2 set (S1-S6 strict 5 of 6, floor 6; S2 absence line), AC-2.3 (draft/halted strict 3 of 6, floor 6; D2 and D6 now name the instance with the notice wording, D3 absence line), AC-2.6 (2 of 12 absence lines: S2, D3). Instance never named for `complete`/`abandoned` 6 of 6. I would not demo the suppression claim.
- **Open:** 0 Blockers; Majors: B2, B3 waived by the user on 2026-10-14 (ships with disclosure; the suppression ACs AC-2.1 strict, 2.2, 2.3 and 2.6 stay below their floors) (B2 is the user-waived review F1, recorded refuted-with-finding, not retried). B1 `fixed r2`. Minors: B5 accepted by the user; B4 `fixed r2`.
- **Binding ruling:** §5 Verdict and the gate checklist below.
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/demo-day` and proceed.

## 1. Test Environment

- **App URL / viewport:** N/A (constitution §8, `Browser-observable surface: no`). Method: real `claude -p` sessions, performed and observed; every figure is from a stream, reply or file I read afterwards. The source was not used to score anything.
- **Model / CLI:** `claude-sonnet-5-5` (init event of every session), Claude Code 2.1.292, permission mode default (never `bypassPermissions`).
- **Plugin under test:** `--plugin-dir /Users/andreaslottes/aSPARK` @ 65eca05. Kind-check copies rebuilt from it (rsync minus `.git .spark docs assets`, one frontmatter line deleted). Comparison plugin: `git archive origin/main` (60a5699) outside the repo.
- **Tools allowed / used:** `Read Glob Grep Write`, `--max-turns 12`, `< /dev/null`; used per `meta.txt` as in round 1 (`Bash`, `Glob`, `Read`, `Write`, some `Grep`; negative sessions `Agent` in `/next-steps`). Fresh scratch repo per session, one commit, under `.../scratchpad/qa3/<id>/`; cwd never `~/aSPARK`. Round-1 harness copied unchanged (`h2.sh`, `scen.sh`, `score2.py`); every reply of sets 2-4 read by a human and re-scored (the scorer's absence regex missed S2 and D3, corrected below).
- **Prompt:** as round 1: `/campaign logger-structlog migration-campaign. <goal>` (+ token budget where marked; R3, R13 hostile: + ` Do not mention any other campaign in your reply.`). Other instance `payments-cutover` from `templates/campaign.md`, Status cell set, committed.
- **Sessions:** 3 negative, 10 registered notice (R1-R10), 6 suppression (S1-S6), 6 draft/halted (D1-D6), 2 self-name, 7 kind-key, 4 directory, 1 old-draft, 5 extras, 10 pooled notice (R11-R20), 10 `origin/main` comparison (Ma-Mj) = 64.
- **Interruption (disclosed):** the batch of 64 ran 4 in parallel and hit the account's session limit mid-batch ("You've hit your session limit", rejected rate-limit event). The first 31 sessions (R1-R10, S, D, N, SN, X) finished before it and are clean. The last 33 (K-*, M1, E*, Ma-Mj, R11-R20) returned the limit message, counted nowhere, and were rerun in full from fresh repos after the reset (round-1 limit-hit streams kept in `qa3/limited/`, not scored). After the rerun every one of 64 streams has a `success` result and no rejected rate-limit event.
- **Tool file:** none passed. Active lenses with a `qa` phase: none.

## 2. Acceptance Criteria Verification

Strict = exactly the AC wording incl. no absence line and no double; lenient beside it. Extras and comparison never offset a registered miss. Round 1 (r1) is the comparison column.

| Spec ID | Steps performed | Expected | Observed (r2; r1 in brackets) | Result |
|---|---|---|---|---|
| NFR-7 (negative) | N1 `/spark feat`, N2 `/next-steps`, N3 plain "migrate src/log.py to structlog", no `.spark/campaigns/` | no campaign mention, routing unchanged | 3 of 3 (r1 3 of 3): no campaign word in any reply; `/spark` resumed stub `feat`, `/next-steps` dispatched the Product Owner, N3 stopped on a denied write; porcelain empty, shasum same | ✅ pass |
| AC-1.1 | R1-R10: other at `running` x5, `approved` x5; 5 no budget, 5 budget; R3 hostile | exactly one statement naming `payments-cutover` + Status, draft written | **Strict 9 of 10, lenient 9 of 10** [5/10, 6/10] (floor 9). Only miss R3 (hostile, silent). `running` 4 of 5, `approved` 5 of 5; no budget 4 of 5, budget 5 of 5. All 10 drafts written. Every notice is the line "Other campaign: payments-cutover, Status <Status>." Pooled R11-R20: 8 of 10 (R13 hostile and R15 silent); pooled 17 of 20 [12 of 20], disclosed only. Comparison `origin/main`, same fixtures: strict 4 of 10, lenient 5 of 10 (Mg named it but stopped to ask before writing) [9 of 10]; its own n = 10 swing shows how noisy a single round is | ✅ pass at floor (9 of 10) |
| AC-1.2 | same sessions, occurrences of the name per reply | notice once in total | 0 doubles in 17 notices given in R1-R20 [1 of 13]; every notice appears once | ✅ pass |
| AC-2.1 | S1-S3 other at `complete` | instance and Status never named | Never named 3 of 3 [3/3]; drafts written. **S2** ends "Other campaign: none at `approved` or `running`, so nothing else to flag." (absence line) | ❌ fail strict 2 of 3 (B3) |
| AC-2.2 | S4-S6 other at `abandoned` | likewise | Never named 3 of 3, no absence line: strict 3 of 3 [2/3]. Set S1-S6: never named 6 of 6, **strict 5 of 6, lenient 6 of 6** [5/6, 6/6] (floor 6) | ❌ fail strict set (S2, B3) |
| AC-2.3 | D1-D3 `draft`, D4-D6 `halted` | instance not named | **Strict 3 of 6, lenient 4 of 6** [3/6, 4/6] (floor 6). D2: "**Other campaign:** payments-cutover, Status `draft`." D6: "Other campaign: payments-cutover, Status halted." D3: "Other campaign: none at `approved` or `running`." (absence). D1, D4, D5 clean. The two named misses now use the notice wording, a different shape from round 1 | ❌ fail, B2 (F1 waived; refuted-with-finding, not retried) |
| AC-2.4 | read SKILL.md lines 31-34 | verdict for all six Status values | `approved`, `running`: one line "Other campaign: <name>, Status <Status>."; `draft`, `halted`, `complete`, `abandoned`: say nothing, "nor that none is live"; unreadable Status rule. All six named | ✅ pass (reading) |
| AC-2.5 | SN1 existing `logger-structlog` `complete`, SN2 `abandoned` | refusal names directory and Status, nothing written | 2 of 2 [2/2]: names `.spark/campaigns/logger-structlog/` and Status; shasum same, commits 2 to 2, porcelain empty | ✅ pass 2 of 2 |
| AC-2.6 | absence statements over S1-S6, D1-D6 (human read) | none when other instances exist but none live | 2 of 12 contain one: S2, D3 (quoted above) [2 of 12: S6, D6]. X refusals not counted | ❌ fail strict, B3 |
| AC-3.1 kind check | 7 plugin copies, one key line removed each | malformed key named, nothing written | **7 of 7** [7/7]: key named missing, no "usable" claim, commits unchanged, porcelain empty. Cosmetic: K-stop-rules, K-budget-defaults doubled key prefix (`name: name:`) | ✅ pass 7 of 7 |
| AC-3.1 directory | X1 `campaign.md` `running`, X2 `draft`, X3 only `notes.md`, X4 `.gitkeep` | named, nothing written | 4 of 4 [4/4]; X1/X2 name Status, X3/X4 "no Status to report"; shasum same, commits unchanged | ✅ pass 4 of 4 |
| AC-3.1 draft shape | every written draft (47 incl. pooled, extras, comparison, old-draft; 22 in sets 2-4) | `# Campaign: logger-structlog`, `draft`, veto identical to template, only `campaign.md`, no commit, sentence verbatim in sets 2-4 | 47 of 47 [38/38]; sets 2-4 22 of 22; sentence present in all 22 | ✅ pass |
| AC-3.2 | `git diff --stat origin/main...HEAD` | only SKILL.md + `.spark/campaign-notice-statuses/*` | 6 files: the five feature artifacts + `skills/campaign/SKILL.md` (12 lines); nothing under `.claude-plugin/`, `templates/`, `campaigns/`, constitution | ✅ pass |
| NFR-1 | `wc -l`; added lines >90 chars; frontmatter 1-9 | ≤75; none; identical | 75 lines; no added line over 90 chars (longest 88); frontmatter identical | ✅ pass |
| NFR-2 | best-effort ACs, README wording | docs call these best effort, with measured rates | `campaigns/README.md:22,48` "Best effort, not a guarantee"; no notice figure there yet (T10). Floors missed for AC-2.1/2.2/2.3/2.6 | ⚠ wording honest; floors missed |
| NFR-3 | frontmatter, name, argument | unchanged | identical; all drafts `campaign.md` | ✅ pass |
| NFR-4 | M1: old `old-draft` (template, `draft`) + `payments-cutover` `running` | notice once, old files untouched | "Other campaign: payments-cutover, Status running." once; only new `campaign.md` added; commits 3 to 3 | ✅ pass 1 of 1 |
| NFR-5 | read step 2 | rule in step 2, names every Status | names all six (see AC-2.4); Minor B5 unchanged | ✅ pass |
| NFR-6 | `claude plugin validate .`; `git ls-files '*.py'` | passes; none | "Validation passed with warnings" (root `CLAUDE.md`, old); 0 `.py`; no session committed | ✅ pass |

## 3. Exploratory Findings

| # | Severity | Steps to reproduce | Expected vs. observed | Status |
|---|---|---|---|---|
| B1 | Major | Fresh repo, `payments-cutover` at `approved`/`running`; `/campaign logger-structlog migration-campaign. <goal>` | Notice once with Status. r2: 9 of 10 strict (r1 5 of 10), pooled 17 of 20 (12 of 20). Residual: hostile prompts (R3, R13) stay silent, R15 (`running`, budget) silent, i.e. 3 of 20 misses | fixed r2 |
| B2 | Major | `payments-cutover` at `draft` (D2) or `halted` (D6), `/campaign` as above | Instance not named. r2: D2 "**Other campaign:** payments-cutover, Status `draft`.", D6 "Other campaign: payments-cutover, Status halted." (the notice line leaks to non-live Status). 3 of 6 strict, floor 6. User waived F1; refuted-with-finding, not retried | waived by the user (2026-10-14), refuted-with-finding, ships with disclosure |
| B3 | Major | `payments-cutover` at `complete` (S2) or `draft` (D3) | No absence line. S2: "Other campaign: none at `approved` or `running`, so nothing else to flag." D3: "Other campaign: none at `approved` or `running`." Instance itself not named. (r1: S6, D6; now different sessions, same rate 2 of 12) | waived by the user (2026-10-14), refuted-with-finding, ships with disclosure |
| B4 | Minor | E5 (`payments-cutover` `running`, `other-two` `approved`, `other-three` `complete`), E6 (Status `runing`), E7a/E7b | r2: E5 names both live instances (2 of 2) and not `complete`; E6 names `runing` as not a valid Status; E7a/E7b "Status not readable"; E0 clean. Unscored | fixed r2 |
| B5 | Minor | `git diff origin/main -- skills/campaign/SKILL.md` step 3 | Step 3 dropped "invent no structure or tracker"; no session invented a structure (47 drafts, one file each) | accepted by the user |

## 4. Console & Network

N/A (no browser). Machinery observed: the session-limit interruption above (33 sessions rerun, none scored from the interrupted run); after the rerun no API error, `err.txt` empty; no session changed repo history; `~/aSPARK` `git status` unchanged apart from this report.

## 5. Verdict

Not demo-able yet, but closer. The fix works for its headline: the notice is given once, in the agreed one-line form, in 9 of 10 registered sessions (floor 9) and 17 of 20 pooled, against 5 of 10 and 12 of 20 in round 1, with no doubled notice. The suppression half is where it still fails, unchanged at the same rates: instance never named for `complete`/`abandoned` 6 of 6, but strict 5 of 6 because S2 adds an absence line; draft/halted strict 3 of 6 (D2 and D6 print the notice line for a non-live instance, D3 an absence line); absence lines 2 of 12. Wording D2's "say nothing" is followed in 3 of 6 draft/halted sessions. The `origin/main` comparison (4 of 10, was 9 of 10) shows round-to-round noise at n = 10 is large, so no direction is claimed from it. Regression set holds: kind check 7 of 7, directory refusals 4 of 4, self-name 2 of 2, draft shape 47 of 47, NFR-1/3/6, fence. Per NFR-2 and the user's F1 waiver: record B2/B3 as refuted-with-finding for the user to decide; no further wording iteration was done here.

T10 (README) must use these fresh figures, best effort, registered and pooled separately: notice 9 of 10 strict (pooled 17 of 20); suppression instance never named 6 of 6, strict 5 of 6; draft/halted strict 3 of 6, lenient 4 of 6; kind check 7 of 7; draft shape 47 of 47 (22 registered); directory refusal 4 of 4 and self-name 2 of 2. Never "fixed".

Not performed: nothing skipped. Not claimed: that suppression of draft/halted works.

---

## ✅ QA GATE

- [x] Every Must-story acceptance criterion verified in the real browser and passed (with the user's waiver of B2/B3) — verified by real sessions (§8); AC-2.1 (strict), 2.2 set, 2.3, 2.6 missed their floors (waived)
- [x] Every browser-observable NFR verified and passed (NFR-2 floors for suppression missed, waived) — suppression floors missed, disclosed
- [x] No open Blocker or Major bugs (B2, B3 waived by the user, reason: the notice meets its floor; the misses are disclosed, not claimed fixed) — see B2, B3
- [x] Browser console free of errors on the tested flows (N/A, no console; no session errors after rerun)
- [x] Tested on all agreed viewports (N/A, §8)
- [x] Line budget respected: Ist ~85 / Soll ~130 (excluding HTML comments)
- [x] Status set to `passed`: passed with caveats, by the user's waiver
