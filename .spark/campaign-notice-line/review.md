# Review Report: campaign-notice-line

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | `origin/main...HEAD` (`4966076..3770297`), `.spark/campaign-notice-line/plan.md` |
| **Status** | `passed` |
| **Round** | 2 |
| **Date** | 2026-10-20 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** F3 and F4 confirmed fixed, `SKILL.md` unchanged and sound; F1's deviation is now documented but not unambiguously accepted: the recorded "offered default" did not exist (F8), so the user must rule explicitly.
- **Open:** `2 open` — Blockers: none; Majors: none (Minors F1, F2: both routed to `/demo-day` and its fix round; F8 `fixed`)
- **Binding ruling:** §6 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

- **Reviewed (round 2):** fix commits `dfaec85` (`evidence.md`, `review.md`, `plan.md`) and `3770297` (`plan.md` §1). `git diff --stat a7b9a4e HEAD -- skills` is empty: `SKILL.md` unchanged since round 1. `origin/main` still `4966076` = merge-base, branch not stale. Working tree: only `.spark/.guard/` untracked.
- **Re-derived from primary source (condition a, verifying fixes F1, F3, F4):** T4 quote compared with `SKILL.md:65-68` and every contract ref with the file; the 38 T5 rows regenerated with `score3.py` from the raw streams (Status per session read from each scratch repo's `payments-cutover/campaign.md`); `Bash` calls recounted over all 76 streams and grepped for writes; the F1 ruling read in the parent session transcript (`05980c0d….jsonl`, user turn 2026-10-20T08:22:30Z and the question before it).
- **Re-derived (condition b, Must AC / NFR-1, NFR-6):** `wc -l` 75; D3 width awk over added lines empty (over-90 only `:53`, `:60`, `:73`, pre-existing); frontmatter `:1-9` identical to `origin/main`; `--diff-filter=A` adds only the four feature artifacts; nothing under `.spark/constitution.md`, `templates`, `campaigns`, `.claude-plugin`, `README.md`; `claude plugin validate .` passes (pre-existing root `CLAUDE.md` warning).
- **Cited, not re-derived:** round 1's W1/W2 rates and AC-2.3 reading (§4), NFR-7 (`evidence.md` T2a, T9): no condition (a)-(d) applies beyond the rows regenerated above.
- **Tool:** `aspark-graph` not re-run; round 1 showed all paths in `unknown_files`, and no indexed file changed since.
- **Not reviewed:** T10, `todo` until `/demo-day`.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ✅ | Spec and plan committed first (`33c06bf`), branch cut from current `origin/main` |
| T2 | ✅ | Negative, baseline, fixture recorded |
| T3 | ✅ | W1 = plan candidate verbatim; D3 trim (i) only, its ban still in "offer neither" `:69` and Never `:73` |
| T4 | ✅ | Verdict table quotes W2 `:65-68` verbatim; refs `:36-44`, `:63-69`, `:27-30` match (F3 fixed r2) |
| T5 | ✅ | 38 per-session rows match the scorer output exactly (F4 fixed r2); harness line over-claimed, corrected (F7) |
| T6 | ⚠️ | DoD (`plan.md:129`) still expects "Status not readable" for `runing`; covered by the deviation note `plan.md:98`, acceptance pending (F1, F8) |
| T7 | ⚠️ | Kind check 6 of 7 (F2) |
| T8 | ✅ | Unchanged since round 1 |
| T9 | ✅ | Fence re-run by this review |
| T10 | — | `todo` by design |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Minor | `skills/campaign/SKILL.md:66-68` | Plan D2(e) and spec A5 say an "unreadable **or unknown/unrecognised**" Status shows "Status not readable". The shipped text names only "unreadable" and prints every non-live Status "as written", so `runing` is printed as written (T6's A5 session). Not live still holds by the `approved`/`running` partition. r2: the deviation is now documented at `plan.md:98` and accurately (`:66-68`, A5 session, D4 bar re-checked); acceptance is not established (F8), and spec A5 still states the opposite. Fix: the user rules explicitly (accept "as written, not live", or route a wording fix to `/demo-day`). **r2 ruling (user, 2026-10-20, explicit): "Wording-Fix in die /demo-day-Runde"**; superseded after QA round 1 (user, 2026-10-21): deferred to a follow-up feature, see `plan.md:98`. | open |
| F2 | Minor | `evidence.md` T7; `SKILL.md:36-44` (unchanged) | AC-3.1 kind check in development is 6 of 7 against the parent's 7 of 7: `K-budget-defaults` wrote a draft and opened with no kind check at all. Step 4 is not in the diff, and 6 extra sessions (3 branch, 3 `origin/main`) held, so it is probably noise, but extras cannot offset it (NFR-2). Minor because development figures do not decide AC-3.1; QA's registered regression set does. Fix: `/demo-day` re-runs the 7; a repeat miss is an AC-3.1 failure, `refuted-with-finding`. | open |
| F3 | Minor | `evidence.md:78-101` (T4) | The AC-2.3 verdict table quotes W1's step 8 (", never live" after unreadable), which W2 removed; plan §4 requires re-deriving it after every restatement. The contract table mixes line numbers (kind check `:38-46` is pre-change, now `:36-44`; step 8 `:65-71` is now `:63-69`). This review re-derived AC-2.3 on W2 (§4): it still holds. Fix: requote W2 in the table and correct the refs. | fixed r2 |
| F4 | Minor | `evidence.md:107-147` (T5) | T5's DoD asks for one row per session incl. tools; the evidence gives set summaries only, and never says sessions called `Bash` (read-only `ls`/`grep`/`cat`, auto-allowed though `--allowedTools` lists `Read Glob Grep Write`; `meta.txt` shows `tools=Bash,Glob,Read,Write`). Fix: add the per-session table or state the deviation, and disclose the `Bash` calls in the harness line. | fixed r2 |
| F5 | Minor | `evidence.md:160` | Draft counts said 40 of 40 and 38 of 40; the listed IDs are 42 drafts (22 W1, 20 W2). Re-derived from the drafts: 42 of 42 structural, sentence 40 of 42. No verdict changes. Corrected in place by the reviewer. | fixed r2 |
| F6 | Minor | `plan.md:14` | Handoff said `10 tasks not done` while T1-T9 are `done`. Corrected to `1 task not done` (T10). | fixed r2 |
| F7 | Minor | `evidence.md:10` (harness line) | The F4 fix said "every session's `meta.txt` shows `tools=Bash,Glob,Read,Write`" and "3 `git` (one `git log`, one `git ls-files`)". Raw streams: 45 of 76 show that set, 31 another mix; `N2`/`M1N2` also called `Agent` (`aspark:product-owner`), undisclosed; `git` subcommands are 2 `log`, 2 `ls-files`, 1 `show --stat` (the 3 was a first-word count). No write anywhere, so no verdict changes. Corrected in place by the reviewer. | fixed |
| F8 | Minor | `plan.md:98` | The note records the ruling as "ok mache weiter" "on the offered default". The transcript shows the user wrote "ok mache wewiter" in reply to an either/or question ("accept as a deviation in plan §1, *or* a wording fix in the `/demo-day` round?") with no default marked, asked alongside F2. "Carry on" picks neither option, so the plan claims a ruling the user did not clearly make; it decides the gate's deviation box. Fix: ask the user the F1 question again with both options and record the verbatim answer; correct the quote and drop "offered default". Not decided by this review. | fixed |

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1.1 | `SKILL.md:65-66` | ✅ met in code; dev 9/10 strict (W2), QA decides |
| AC-1.2 | `SKILL.md:31` (silent read), `:65` | ✅ name_in_reply = 1 in every hit |
| AC-1.3 | `SKILL.md:65` | ✅ dev 3/3 after reading Y1/Y2 |
| AC-2.1 | `SKILL.md:66-67` | ✅ dev 6/6, no false "live" |
| AC-2.2 | `SKILL.md:66` "Status as written" | ✅ dev 6/6 |
| AC-2.3 | `SKILL.md:65-68` | ✅ re-derived on W2: `approved`/`running` live; `draft`/`halted`/`complete`/`abandoned` named "never live"; unreadable → "Status not readable", not live by the partition (form gap in F1); `evidence.md` T4 now quotes the same text r2 |
| AC-2.4 | `SKILL.md:29-30` (unchanged) | ✅ G1, G2, X1, X2, T8b |
| AC-3.1 | unchanged steps 4, 7, 8 | ⚠️ partial in dev: kind check 6/7 (F2); sentence 40/42, W2 20/20 |
| AC-3.2 | diff fence | ✅ (README pending T10) |
| NFR-1 | `SKILL.md` | ✅ 75 lines, width awk empty |
| NFR-2 | `evidence.md` | ✅ every rate labelled *development*, A3 gap restated, P4 generosity disclosed |
| NFR-3 | — | ✅ no command, argument, file or template change |
| NFR-4 | — | ✅ T8 re-derived |
| NFR-5 | `SKILL.md:31`, `:63-69` | ✅ emitting rule in step 8 only |
| NFR-6 | — | ✅ validate passes, no new tracked file outside the feature dir |
| NFR-7 | — | ✅ cited (`evidence.md` T2a, T9) |

## 5. What Was Checked

- [x] Correctness: step 2 is read-only; step 8 gives all six Statuses a verdict; closing sentence intact; D3 trim's ban still lives at `:69`, `:73`
- [x] Non-functional: line cap, width, frontmatter, fence, validate; library lens §1/§2/§4: no surface change, semver effect explicit (D7, user ruled 0.14.3), contract text intact
- [x] Error handling: refusals unchanged; unreadable Status handled, unknown Status per F1
- [x] Round 2: 38 T5 rows regenerated (identical), 76 streams' `Bash` calls grepped for writes (none), F1 ruling read in the transcript
- [x] Security: no executable code; harness never used `bypassPermissions` (`permissionMode: default` in streams)
- [x] Tests: no suite (constitution §4); development streams re-scored from source
- [x] Readability: one rule in one step; the slot form makes "live" part of the template

## 6. Verdict

"The code is unchanged since round 1 and still does what the spec asks; the structural checks re-run clean (75 lines, no added line over 90 characters, frontmatter identical, fence intact, validate passes). F3 and F4 are fixed: the T4 table quotes the shipped W2 text with correct line references, and all 38 per-session rows match what the scorer produces from the raw streams. The F4 harness line over-claimed the tool sets and omitted two `Agent` calls in negative runs; I corrected it (F7), and no verdict depends on it. The review does not pass yet for one reason: F1's deviation is now documented accurately, but its acceptance rests on 'ok mache wewiter', written in reply to an either/or question with no default, which the plan records as acceptance of an 'offered default' that was not offered (F8). That answer may well mean acceptance, but this review cannot assume it; the user needs to answer the F1 question explicitly, after which the note's quote is corrected and the gate can pass without another code change. F2 stays open as a Minor for `/demo-day`'s registered kind-check set."

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place — never duplicate it as a second gate.*

- [x] No open Blocker findings
- [x] No open Major findings (or explicitly waived by the user, with reason recorded here)
- [x] Every Must AC traces to implementing code; no constitution non-negotiable violated
- [x] All plan deviations documented and ruled (F1 documented at `plan.md:98`; user ruled explicitly: wording fix routed to the fix round after `/demo-day`, F8 closed)
- [x] Test suite runs green (no suite by constitution §4; `claude plugin validate .` passes, structural checks re-run)
- [x] Line budget respected: Ist 104 / Soll ~150 (excluding HTML comments)
- [x] Status set to `passed` (user approved the gate, 2026-10-20)
