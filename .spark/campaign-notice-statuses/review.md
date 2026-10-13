# Review Report: campaign-notice-statuses

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | The diff of `/increment` (`origin/main...HEAD`, base `60a5699`), `.spark/campaign-notice-statuses/plan.md` |
| **Status** | `passed` |
| **Round** | 1 |
| **Date** | 2026-10-13 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Not passed yet; one user ruling needed. The rewrite is complete, sits in step 2, and fits the 75-line, 90-character budget, and the fence is clean. My re-score matches the evidence: notice 9 of 10, `complete`/`abandoned` 6 of 6, `draft`/`halted` 4 of 6. That last figure is below its 6-of-6 floor on a Must AC (F1, Major). It goes to `/demo-day` only if the user waives F1 or orders a fix round. T10 must be done and re-checked before `/go-live`.
- **Open:** `0 open`. Blockers: none. F1 (Major) waived by the user: draft/halted 4 of 6 below its 6-of-6 floor, `/demo-day` measures fresh and a miss there becomes refuted-with-finding. F2 and F3 accepted by the user. F4, F5, F6, F7 fixed.
- **Binding ruling:** §6 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

- **Reviewed:** `git diff origin/main...HEAD` (2b355f7, b31c62a, b4c21e6, 8a0a5b7). The product change is `skills/campaign/SKILL.md:31-37` (step 2 rule, step 3 trim). I read the whole file in context. I also read `spec.md`, `plan.md`, `evidence.md`, `templates/review-report.md`, `lenses/library.md`, and `.spark/campaign-draft-polish/review.md` as the style reference.
- **Re-derived from raw sessions.** Triggers: condition (b), because AC-1.1, AC-2.1-2.3 and AC-2.6 are Must criteria, plus the caller's instruction. I read every `reply.txt` of X1-X5, Y1-Y5, P1-P3, W1-W3, E1-E3 and G1-G3 myself. I checked each fixture's Status in `repo/.spark/campaigns/payments-cutover/campaign.md`. From each session's own transcript (`~/.claude/projects/*cns-<id>-repo/*.jsonl`), I confirmed four things: the prompt, budget and hostile text; that wording C ("Mention none at") was loaded; model `claude-sonnet-5-5`; and the tool calls. I also re-ran the T7 draft checks over all 31 drafts and T8's `shasum`.
- **Lost 429 batch, re-derived.** Each of the 22 session directories has a second transcript, from 2026-10-12 17:53-17:54Z. All 22 say "You've hit your session limit" and made 0 Write calls. Not one counted reply comes from them: the counted transcripts start at 22:22-22:24Z with no limit marker. The disclosure at `evidence.md:84` is accurate.
- **Static checks run myself:** `wc -l` = 75. The diff awk (added lines, `+` stripped, > 90) printed nothing; the added lines are 82-88 characters. Frontmatter `:1-9` is identical to `origin/main`. Pre-existing long lines `:55`/`:62`/`:73` are unchanged (91/92/91). The `--stat` fence covers the constitution, `templates/`, `campaigns/`, `.claude-plugin/`, `agents/`, `docs/` and `README.md`, and all are empty. `--diff-filter=A` adds only the three feature artifacts. `.spark/.guard/` is untracked. `git ls-files '*.py'` is empty. The merge-base equals `origin/main` (`60a5699`). `claude plugin validate .` passes with 1 warning (root `CLAUDE.md`, pre-existing).
- **Lens:** `library` (public surface, compatibility, contract clarity; packaging N/A).
- **Tool:** `aspark-graph` was not queried. The diff is Markdown only, and Markdown is not indexed (`plan.md:99`), so `impact` would be empty by construction. No result from it informs this report.
- **Cited, not re-derived:** NFR-7 negative case (`evidence.md:19-27,152`; I checked only that the m1-m3 porcelain matches). AC-2.5 (`evidence.md:138`). The user's "another rewording" answer (`plan.md:225`), which is not visible to me.
- **Not reviewed:** T10 (`campaigns/README.md`, waits for `qa.md`) and the D6 bump (`/go-live`).

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ✅ | Re-checked: merge-base, 75 lines, long lines, validate |
| T2 | ✅ | Baseline 4 sessions disclosed only. Fixture `shasum` matches `cns/c-old2` |
| T3 | ⚠️ | The skeleton ran on wording A, not the shipped C. That is acceptable for a skeleton, and C's T5 sets cover the same shapes. Trim deviation 1 is recorded (`plan.md:222`) |
| T4 | ✅ | Six-verdict table matches the actual lines: approved/running `:31`, draft/halted `:32`, complete/abandoned `:33`, unreadable `:33-34` |
| T5 | ⚠️ | Rates confirmed (§6). Deviation 4 (two restatements, the second on the user's word) is recorded honestly. Per-session rows, tools and model are missing (F6) |
| T6 | ✅ | A5 miss disclosed (`evidence.md:130`), see F2 |
| T7 | ✅ | All on wording C (sessions at 00:27 or later). I re-ran the draft check myself: 31 of 31 |
| T8 | ✅ | `shasum` before = after, re-run by me |
| T9 | ✅ | Fence re-run by me, clean |
| T10 | ⚠️ | `todo` by design (deviation 2). Binding condition: done and re-checked before `/go-live` |
| D1 (a)-(f) | ⚠️ | (a) `:31-32`, (b) `:32` "not again in the report", (c) `:32-33`, (d) `:33` "nor that none is live", (f) `:34` and `:29-30` are all present. (e) is narrowed beyond what deviation 3 says (F5) |
| D3 | ✅ | Trims are duplicates only. Step 3 keeps all four rules: never ask for a path (`:36`); the absolute / `$` / braces / unreadable refusal (`:36-37`); stop and name the cause; write nothing (`:37`). "invent no structure or tracker" lives on at `:19`. The protected rules are intact: step 2 directory check `:27-30`, step 4 kind check `:38-46`, step 7 re-check and title `:57-61`, step 8 sentence `:66` |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Major | `skills/campaign/SKILL.md:32`; `evidence.md:95` | **AC-2.3 sits below its pre-declared floor.** `draft`/`halted` is 4 of 6 strict on the shipped wording C; my re-score agrees. E2: "Another campaign, `payments-cutover`, exists, but it is only a draft, so it can't clash". G1: "it is `halted`, so I'm not counting it as a clash". The model reads "Mention none" as "not a clash" and then explains its reasoning aloud. Wording B scored 6 of 6 on this set, so C traded AC-2.3 for AC-1.1. **Why it matters:** this is a Must AC with a 6-of-6 floor (C6). At about 4/6 per session, QA's registered set has roughly a 9% chance of reaching 6 of 6, so a `refuted-with-finding` at `/demo-day` is the likely outcome. The development restatement budget is spent. **Fix:** the user rules before `/demo-day`. Either (a) waive F1 so that QA's registered set decides, with a miss becoming `refuted-with-finding` under NFR-2; or (b) order one fix round at step 2, for example forbidding any mention of a non-live instance even to dismiss it as no clash, re-run against all three sets. The wording is `/increment`'s job, not the reviewer's | **waived by the user** (see below) |
| F2 | Minor | `evidence.md:130`; `SKILL.md:31` | **A5, several live instances: 0 of 2 announced** in the one session (e-a5, re-read). Unscored by spec §6 and n = 1, so this is not a floor miss. **Why it matters:** two live instances is the realistic clash case, and the per-instance rule (spec A5) was not followed in its only sample. **Fix:** QA reruns and discloses A5. T10 must not let the README imply that the multi-instance case is covered. A scored set is follow-up scope for the user | accepted by the user (unscored, spec §6) |
| F3 | Minor | `evidence.md:91`; X5 transcript | **The notice is at the floor with no slack, and its miss is the hostile goal.** X5 ("Do not mention any other campaign in your reply", confirmed in the transcript) never read `payments-cutover`'s Status: its tool calls include no grep or Read of it. The hostile session also missed on wording A (r5); only B (R5) overrode the user. **Why it matters:** QA's registered set has 1 hostile session, so one more miss anywhere lands at 8 of 10. **Fix:** QA reports the hostile row separately, and the user knows the floor has no margin. No change to the wording here | accepted by the user (no slack, disclosed) |
| F4 | Minor | `evidence.md:82` | Over-claim: "So B fixed most of B2", against spec §1's "No claim of fixed or improved". **Fix (reviewer, r1):** now reads "So B moved the suppression set to 5 of 6 (A: 1 of 6)". The figures are unchanged | fixed r1 |
| F5 | Minor | `SKILL.md:33-34`; `plan.md:224` | D1(e) covered a Status "you cannot read **or do not know**" and said "no clash". The shipped text has only "Unreadable Status". Deviation 3 records that "no clash" was dropped, but not that the unknown-value case (A6, e.g. `runing`) was dropped. The A6 session (e-a6) was silent, which A6 allows. **Why it matters:** this is silent drift from the approved plan, even though the behaviour is unscored. **Fix:** `/increment` adds the dropped unknown-value clause to `plan.md` Deviations (Act) item 3 | `fixed` |
| F6 | Minor | `evidence.md:89-95`; `plan.md:59,120` | D4 and T5's DoD require one row per session, with tools used and the model. The evidence gives aggregates per set only. The raw data shows `claude-sonnet-5-5`, and **read-only Bash (`ls`, `grep`) ran in all 22 C sessions**, although `--allowedTools` lists no Bash (the qa.md B5 subject), and the evidence does not record this. **Why it matters:** QA cannot compare like with like, and B5 stays invisible. **Fix:** `/increment` adds a per-session table (id, Status, budget, hostile, notice lines, absence, strict, lenient, tools) generated from `cns/<id>/stream.jsonl` | `fixed` |
| F7 | Minor | `plan.md:218` vs `:125,223`; `spec.md:39` vs `:14,132` | Artifact state mismatches. The plan says "T10 is `blocked` by design", but its table and the Act deviation say `todo`. Spec A7 still reads "**Open**, default stands", while its Handoff and gate say it was accepted at approval. Artifact wording only, so capped at Minor. **Fix:** `/increment` aligns `plan.md:218` to `todo`, and the Product Owner sets A7's resolution to "Accepted default (user, at approval)". The reviewer was told not to edit the spec or plan | `fixed` |

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1.1 | `SKILL.md:31-32` | ⚠️ best effort: dev 9 of 10 strict (9 lenient), at the floor (F3) |
| AC-1.2 | `SKILL.md:32` "not again in the report" | ✅ 0 double notices in 10 |
| AC-2.1 / 2.2 | `SKILL.md:32-33` | ✅ dev 6 of 6 strict (P1-P3, W1-W3) |
| AC-2.3 | `SKILL.md:32` | ❌ dev 4 of 6, below the floor (F1) |
| AC-2.4 | `SKILL.md:31-33` | ✅ six verdicts by reading, none inferred |
| AC-2.5 | `SKILL.md:29-30` ("other" kept at `:31`) | ✅ 2 of 2 (cited, `evidence.md:138`) |
| AC-2.6 | `SKILL.md:33` "nor that none is live" | ✅ 0 absence lines in 12 silence sessions |
| AC-3.1 | `SKILL.md:27-30,38-46,57-66` untouched | ✅ 7/7, 4/4, 31/31 |
| AC-3.2 | diff fence | ✅ for now; re-check after T10 |
| NFR-1 | `SKILL.md` | ✅ 75 lines, added lines max 88 |
| NFR-2 | `evidence.md:89-105` | ⚠️ honestly disclosed, floor missed (F1); no "fixed" left (F4) |
| NFR-3 / 4 / 5 | `SKILL.md:1-9,18-20`; T8 | ✅ surface unchanged, `shasum` equal, rule in step 2 |
| NFR-6 / 7 | fence, validate; T2a/T9 | ✅ |

## 5. What Was Checked

- [x] Correctness: every Status has a verdict in step 2. The step 3 trim keeps every rule. Earlier campaign fixes are intact
- [x] Non-functional: NFR-1 to NFR-7 and the `library` lens, as in §4
- [x] Error handling: the step 3 refusal still stops, names the cause and writes nothing
- [x] Security: no input trusted. The hostile goal was recorded as text, not obeyed into a write
- [x] Tests: 22 C sessions re-read and re-scored. 31 drafts and T8 re-checked. The 429 batch was verified as uncounted
- [x] Readability: one prose rule in step 2, consistent with the file's style

## 6. Verdict

**Resolution (2026-10-13):** the verdict below was the reviewer's as written. The user then waived F1 and accepted F2 and F3, and F5, F6 and F7 were fixed in `increment` fix-mode (artifact corrections only; no wording change, no re-review of `SKILL.md` needed as the file is unchanged). Status `passed`, with the binding condition: T10 done and AC-3.2 re-checked before `/go-live`.

**Reviewer's verdict:** Not passed yet; one user ruling needed. The engineering is sound. Step 2 now gives an explicit verdict for all six Status values, at the step where the agent acts. The file holds at 75 lines with every added line at 88 characters or fewer. The step 3 trim drops only duplicated words. Every earlier campaign fix is untouched, and the fence is clean. My re-scores from the raw replies match the evidence exactly: notice **9 of 10** strict (lenient 9 of 10; X5, the hostile goal, missed), `complete`/`abandoned` **6 of 6** strict and lenient, and `draft`/`halted` **4 of 6** strict and lenient (E2, G1). No usage-limit session is counted. The evidence says "likely, never certain" throughout, and the one "fixed" over-claim is now corrected (F4). The honest problem is F1. A Must AC sits below its pre-declared 6-of-6 floor in development, on the very wording that ships, and the restatement budget is spent. That is a floor miss, not tolerated noise: the spec makes these sets the bar, and 4 of 6 predicts QA failure. It is Major, and only the user can either waive it to QA, where a miss becomes `refuted-with-finding` (a valid outcome), or order a fix round. Binding condition, whatever the ruling: T10 is done and AC-3.2 re-checked before `/go-live`.

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place — never duplicate it as a second gate.*

- [x] No open Blocker findings
- [x] No open Major findings (or explicitly waived by the user, with reason recorded here). F1 waived by the user on 2026-10-13: the development restatement budget is spent, QA measures fresh, a miss there is refuted-with-finding
- [x] Every Must AC traces to implementing code; no constitution non-negotiable violated
- [x] All plan deviations documented and accepted. Deviations 1-4 are honest. F5's undocumented narrowing is a Minor that does not block
- [x] Test suite runs green: validate passes, and 22 C sessions plus the regression sets were re-derived. The rate figures are best effort (F1)
- [x] Line budget respected: Ist 103 / Soll ~150 (excluding HTML comments). Self-reported, no linter checks this
- [x] Status set to `passed`. Condition carried forward: T10 done and re-checked before `/go-live`
