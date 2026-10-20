# Review Report: campaign-notice-line

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | `origin/main...HEAD` (`4966076..a7b9a4e`), `.spark/campaign-notice-line/plan.md` |
| **Status** | `changes-requested` |
| **Round** | 1 |
| **Date** | 2026-10-17 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** the code change is sound and within every structural constraint; one undocumented plan deviation (F1, unknown Status values) must be documented and ruled before the gate can pass.
- **Open:** `1 open` — Blockers: none; Majors: none (Minor F2: see §3, goes to `/demo-day`; F1, F3, F4 `fixed`, awaiting re-review)
- **Binding ruling:** §6 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

- **Reviewed:** `skills/campaign/SKILL.md` (the only code change, 2 hunks, +7/-7), plus `spec.md`, `plan.md` and `evidence.md` of this feature. Working tree: only `.spark/.guard/` untracked, nothing uncommitted before this review.
- **Re-derived from primary source (condition b, Must AC verification):** `wc -l` = 75; the D3 width awk over added lines prints nothing; the over-90 lines are now `:53`, `:60`, `:73` (the pre-existing ones, shifted up, outside the diff); frontmatter `:1-9` byte-identical to `origin/main`; `--diff-filter=A` adds only the three feature artifacts; `claude plugin validate .` passes (pre-existing root `CLAUDE.md` warning). The development streams under the `/increment` scratchpad (`…/7c365ac8-…/scratchpad/cnl/`) were re-scored with its own `score3.py` plus a read of every flagged reply: W2 notice 9/10, listing 6/6, none 3/3 (Y1, Y2 scorer false flags confirmed by reading); W1 notice 9/10, listing 5/6, none 3/3; 0 limit markers in all scored streams, 2 each in the discarded `Q*-limit` streams. Drafts: 42 of 42 structurally correct, closing sentence 40 of 42 (F5). T8 `shasum` and `## ` count (15/15) and the K-budget-defaults miss re-read from the logs.
- **Cited, not re-derived:** NFR-7 negative runs (`evidence.md` T2a, T9) and T1 branch freshness: no condition (a)-(d) applies.
- **Tool:** `aspark-graph` (CLI). `staleness` → `files_checked: 0`; `impact --diff origin/main...HEAD` → `files: []`, all four paths in `unknown_files`. That is "not indexed", not "no impact"; scoping was done by hand from the diff. `story_trace` not run (feature artifacts are not indexed, constitution §3 defect 1).
- **Not reviewed:** T10 (README figures), intentionally `todo` until `/demo-day`.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ✅ | Spec and plan committed first (`33c06bf`), branch cut from current `origin/main` |
| T2 | ✅ | Negative, baseline, fixture recorded |
| T3 | ✅ | W1 = plan candidate verbatim; D3 trim (i) only, its ban still in "offer neither" `:69` and Never `:73` |
| T4 | ⚠️ | Verdict table quotes W1, not the shipped W2 text; line refs mixed (F3) |
| T5 | ⚠️ | W2 restatement allowed by D4 and documented; no pole trade (verified). No per-session row with tools (F4). W2 omits D2(e)'s "or unknown" (F1) |
| T6 | ✅ | A5 session shows the F1 gap, disclosed |
| T7 | ⚠️ | Kind check 6 of 7 (F2); counts corrected (F5) |
| T8 | ✅ | Re-derived: both entries once, `shasum` equal, 15/15 sections |
| T9 | ✅ | Fence re-run by this review |
| T10 | — | `todo` by design |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Minor | `skills/campaign/SKILL.md:66-68` | Plan D2(e) and spec A5 say an "unreadable **or unknown/unrecognised**" Status shows "Status not readable". The shipped text names only "unreadable" and says "Status as written" for every non-live entry, so a value like `runing` is printed as written, exactly what T6's A5 session did (`other-four (\`runing\`, … not live)`). Not live is still correct by the `approved`/`running` partition. Undocumented deviation; also leaves open whether "Status not readable" replaces `<Status>` inside the parentheses. Fix: since D4 forbids a third development wording, record the deviation in plan §1 and have the user rule on spec A5 (accept "as written, not live", or route a wording fix to the `/demo-day` fix round). | fixed |
| F2 | Minor | `evidence.md` T7; `SKILL.md:36-44` (unchanged) | AC-3.1 kind check in development is 6 of 7 against the parent's 7 of 7: `K-budget-defaults` wrote a draft and opened with no kind check at all. Step 4 is not in the diff, and 6 extra sessions (3 branch, 3 `origin/main`) held, so it is probably noise, but extras cannot offset it (NFR-2). Minor because development figures do not decide AC-3.1; QA's registered regression set does. Fix: `/demo-day` re-runs the 7; a repeat miss is an AC-3.1 failure, `refuted-with-finding`. | open |
| F3 | Minor | `evidence.md:78-101` (T4) | The AC-2.3 verdict table quotes W1's step 8 (", never live" after unreadable), which W2 removed; plan §4 requires re-deriving it after every restatement. The contract table mixes line numbers (kind check `:38-46` is pre-change, now `:36-44`; step 8 `:65-71` is now `:63-69`). This review re-derived AC-2.3 on W2 (§4): it still holds. Fix: requote W2 in the table and correct the refs. | fixed |
| F4 | Minor | `evidence.md:107-147` (T5) | T5's DoD asks for one row per session incl. tools; the evidence gives set summaries only, and never says sessions called `Bash` (read-only `ls`/`grep`/`cat`, auto-allowed though `--allowedTools` lists `Read Glob Grep Write`; `meta.txt` shows `tools=Bash,Glob,Read,Write`). Fix: add the per-session table or state the deviation, and disclose the `Bash` calls in the harness line. | fixed |
| F5 | Minor | `evidence.md:160` | Draft counts said 40 of 40 and 38 of 40; the listed IDs are 42 drafts (22 W1, 20 W2). Re-derived from the drafts: 42 of 42 structural, sentence 40 of 42. No verdict changes. Corrected in place by the reviewer. | fixed |
| F6 | Minor | `plan.md:14` | Handoff said `10 tasks not done` while T1-T9 are `done`. Corrected to `1 task not done` (T10). | fixed |

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1.1 | `SKILL.md:65-66` | ✅ met in code; dev 9/10 strict (W2), QA decides |
| AC-1.2 | `SKILL.md:31` (silent read), `:65` | ✅ name_in_reply = 1 in every hit |
| AC-1.3 | `SKILL.md:65` | ✅ dev 3/3 after reading Y1/Y2 |
| AC-2.1 | `SKILL.md:66-67` | ✅ dev 6/6, no false "live" |
| AC-2.2 | `SKILL.md:66` "Status as written" | ✅ dev 6/6 |
| AC-2.3 | `SKILL.md:65-68` | ✅ re-derived on W2: `approved`/`running` live; `draft`/`halted`/`complete`/`abandoned` named "never live"; unreadable → "Status not readable", not live by the partition (form gap in F1) |
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
- [x] Security: no executable code; harness never used `bypassPermissions` (`permissionMode: default` in streams)
- [x] Tests: no suite (constitution §4); development streams re-scored from source
- [x] Readability: one rule in one step; the slot form makes "live" part of the template

## 6. Verdict

"The change does what the spec asks: step 2 now only reads the other instances' Status and step 8 carries one required line that gives every Status a verdict. It stays at 75 lines with no added line over 90 characters, leaves the frontmatter and the public surface untouched, and the development figures, which I re-scored from the raw streams, match the evidence (notice 9 of 10, listing 6 of 6, none 3 of 3 on W2) and are honestly labelled as development, not a forecast. It does not pass yet for one reason: the shipped wording drops plan D2(e)'s 'or unknown' Status, so a malformed value is printed as written instead of 'Status not readable' (F1). This is an undocumented deviation from the plan and from spec A5. It is low-impact and the user must rule on it, not this review; D4 bars a third development wording, so the fix is a recorded decision, not more editing. F2 (kind check 6 of 7 in development) does not block; QA's regression set decides it. F3 and F4 are evidence gaps to close in the same fix pass. F5 and F6 I corrected myself."

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place — never duplicate it as a second gate.*

- [x] No open Blocker findings
- [x] No open Major findings (or explicitly waived by the user, with reason recorded here)
- [x] Every Must AC traces to implementing code; no constitution non-negotiable violated
- [ ] All plan deviations documented and accepted (F1 open: D2(e) "or unknown" dropped, not documented)
- [x] Test suite runs green (no suite by constitution §4; `claude plugin validate .` passes, structural checks re-run)
- [x] Line budget respected: Ist 100 / Soll ~150 (excluding HTML comments)
- [ ] Status set to `passed`
