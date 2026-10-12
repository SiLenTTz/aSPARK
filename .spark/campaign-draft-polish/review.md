# Review Report: campaign-draft-polish

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | The diff of `/increment` (`origin/main...HEAD`, base `771a1e2`), `.spark/campaign-draft-polish/plan.md` |
| **Status** | `passed` |
| **Round** | 1 |
| **Date** | 2026-10-11 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Pass, with conditions. The three rewordings sit at the steps that carry them out (2, 7, 8). The budget holds at 75 lines and 90 characters. The fence is clean. The dev rates are honest, except AC-1.2, which is 4 of 6 and not 5 of 6 (F1). T11 must be done before `/go-live`.
- **Open:** `5 open`. Blockers: none. Majors: none. Minors F1-F4 and Nit F6 (see §3). F5 is `fixed r1`.
- **Binding ruling:** §6 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

- **Reviewed:** `git diff origin/main...HEAD` (e86a1e4, 785df25, b2b3787, 7d87c07). Product: `skills/campaign/SKILL.md`, checked with the full diff and with `-w --word-diff`. Also the spec set (`spec.md`, `plan.md`, `evidence.md`), `.spark/constitution.md`, `templates/campaign.md`, `campaigns/README.md`, `campaigns/migration-campaign.md` and the parent `.spark/campaign-start/spec.md` (AC-1.4, AC-1.6, AC-1.7, AC-3.3).
- **Re-derived from raw sessions:** I did not take the evidence file's word. I re-derived its claims from the development streams and replies in the session scratchpad (`dp/<id>/{stream.jsonl,reply.txt,porc.txt,commits.txt,repo}`). That covers T7 (m-r1..r5, m-a1..a5), the suppression sessions (s-d1..s-n2), T8 (k-* ×7, x-notes, x-empty), T9 (old, old2, `shasum`) and T10 (p1-p3). I also wrote my own script to check the order of the kind check and the Write call. Trigger: condition (b), because AC-1.1, AC-1.2 and AC-4.1 are Must criteria, plus the caller's instruction.
- **Lens:** `library` (public surface, compatibility, contract clarity; packaging N/A).
- **Tool:** `aspark-graph` was not queried. The diff is Markdown only, and only source files are indexed (`tools/aspark-graph.md:93-95`), so `impact` would be empty by construction. No result from it informs this report.
- **Not reviewed:** `plugin.json` 0.14.1 (D6, `/go-live`'s release commit) and T11 (`campaigns/README.md`, blocked until `qa.md`).

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ✅ | Base `771a1e2` = `origin/main` (re-checked after fetch); `.spark/.guard/` untracked; `*.py` 0 |
| T2 | ✅ | Fixture `shasum`s match the T9 after-state (re-run by me) |
| T3 | ✅ | B6 at step 2, `SKILL.md:31-32`: name **and** Status, once, clash or not, `draft`/`halted` suppressed. See F1 and F3 |
| T4 | ✅ | B4 at step 7, `:60-61`. The verbatim sentence `:58-60` is unchanged and the HTML comment keeps `<campaign-name>` (the AC-2.2 diff re-run on 3 drafts holds) |
| T5 | ✅ | B8 at step 8, `:66`: quoted sentence with the AC phrase in lower case |
| T6 | ✅ | 75 lines (≤ 75); widest line 90 **characters**. `awk` reports 92 bytes at `:62` because of `…`, so check with a character count. Old `:59` (133) is gone. Lines 1-9 are identical. The word diff shows exactly the three edits plus two trims, `(no check was run)` and `; run or offer nothing.`; see F2. B1/B3 wording: only whitespace changed |
| T7 | ⚠️ | The figures re-derive as recorded, except AC-1.2: s-d2 is a miss, not neutral (F1) |
| T8 | ✅ | Kind check 7 of 7 re-derived: `malformed` and the key named, nothing written, commits 1. AC-1.6 16 of 16 and AC-1.7 16 of 16 hold; no session offered to run anything. See F4 for the happy-path order |
| T9 | ⚠️ | Refusal, notice and `shasum` all re-derived OK. The old2 row leaves out its self-admitted "Kind check, which should have come before the write" (F4) |
| T10 | ✅ | p1-p3: 0 campaign mentions, porcelain empty. Fence re-run by me (§4 AC-4.2) |
| T11 | ⚠️ accepted | `blocked` by design (D5, spec A4); the plan was approved this way. Ruled in §6: not a finding, and a hard condition before `/go-live` |
| Deviation | ✅ | The re-wrap also covered step 1 (`:24-26`), not only steps 2-8 as written. The word diff shows whitespace only. Wording corrected (F5) |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Minor | `evidence.md:111,115`; `SKILL.md:32` | **Problem:** the dev rate for AC-1.2 is recorded as 5 of 6. **Ruling:** s-n2 counts as neutral, which is defensible: no other instance exists, so the true statement of absence names nothing. s-d2 is a **miss**: "There's one other campaign instance in this repo at `approved` or `running`? No." mentions that the `draft` instance exists, which `:32` forbids ("never mention one at `draft`"). The evidence's "names nothing, claims nothing" is inaccurate. s-h1 is a false claim, and its "I mention it only if it clashes" is a misreading of "clash or not". The strict dev rate is **4 of 6**. **Why it matters:** this is a Must story with no floor, and the reported figure was 1 too high. **Fix:** set `evidence.md` to 4 of 6. QA should run ≥2 `draft` and ≥2 `halted` sessions and score any mention of a `draft`/`halted` instance's existence as a miss. If misses recur, the user decides (no floor). | open |
| F2 | Minor | `SKILL.md:67-69`; `evidence.md:38,85` | **Problem:** the trim of "run or offer nothing" removes the ban on *offering to run* the next-step session. The evidence says the ban stays in "offer neither", but that phrase refers to approval and commits. "Dispatch nothing" and the Never list cover *running* only. Plan D3 named this phrase as a trim candidate, so the trim conforms to the plan, but the redundancy premise behind it is wrong. Observed: 0 offers in 21 replies (m-*, s-*, t4*, t5*). No AC needs the ban (parent AC-1.7: "runs neither"). **Why it matters:** a rule was dropped silently, against D3's "rule still stated elsewhere". **Fix:** the EM records the trim as accepted and the evidence wording is corrected, or the ban is restored within 75 lines. | open |
| F3 | Minor | `SKILL.md:32`; spec AC-1.2 | **Problem:** suppression names only `draft`/`halted`. `complete` and `abandoned` (`templates/campaign.md:5`) are in neither list, so by contrast the agent may mention them. No session measured this case. **Why it matters:** this is a false-notice path that is not covered. **Fix:** QA adds one `complete` or `abandoned` session. On a mention, reword to "never mention one at any other Status" (same length) and re-measure. | open |
| F4 | Minor | `SKILL.md:39-40,57-58`; `evidence.md:131` | **Problem:** on the happy path, the kind check does not open the reply. In 0 of 37 dev sessions that wrote a draft did the seven-line check come before the Write. m-r5 and old2 emit it *after* the Write; old2 says "should have come before the write". The other 35 never emit it. The step-7 re-check is therefore not followed. The malformed case holds 7 of 7, so AC-4.1 is unaffected. Step 4's words are unchanged (whitespace only), and no baseline streams were kept, so pre-existing vs regression is unknown. **Why it matters:** this rule is followed 0 of 37 times and the evidence does not disclose it. **Fix:** at QA, record the order in 2-3 sessions each on `origin/main` and on the PR head. If only the PR head misses it, raise to Major (regression). Otherwise it goes to the backlog as a B1 follow-up, outside this fence. Add the old2 slip to `evidence.md:131`. | open |
| F5 | Minor | `plan.md:14,27,178`; `evidence.md:38,83,85` | **Problems:** the plan Handoff `Open` said "T1–T11 `todo`" while §3 shows T1-T10 `done` and T11 `blocked`. Branch SHAs `2ba7629`/`2411075` are on no branch (actual: `771a1e2`/`e86a1e4`). The re-wrap was described as "steps 2-8" but was 1-8. The T6 rule-table line ranges were off by one in 6 places. **Fix:** corrected by the reviewer, facts only, no scope change. | fixed r1 |
| F6 | Nit | `evidence.md:9,91`; scratch `dp/score.py` | **Problem:** the dev AC-2.2 scorer checks only removed template lines, exempts any line starting with `- <`, and ignores added lines. My full `diff` of m-r2, m-a3 and s-h1 against the template shows only the permitted differences, so the 10 of 10 is plausible. Separately, the sessions ran Bash `ls` (step 2 asks for it) although the harness line says the tools are "Read Glob Grep Write". **Fix:** QA diffs every draft in full and records the tool set that was actually used. | open |

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1.1 | `SKILL.md:31-32` (step 2) | ✅ wording; dev 9 of 10 re-derived (m-r2 miss), at floor; QA measures |
| AC-1.2 | `SKILL.md:32` | ⚠️ dev 4 of 6 strict (F1); `complete`/`abandoned` gap (F3) |
| AC-2.1 | `SKILL.md:60-61` (step 7) | ✅ dev 10 of 10 re-derived (`head -1`) |
| AC-2.2 | `SKILL.md:58-61`; `templates/campaign.md:10` untouched | ✅ comment identical; full diff on 3 drafts (F6) |
| AC-3.1 | `SKILL.md:66` (step 8) | ✅ dev 10 of 10 re-derived (phrase grep) |
| AC-4.1 | step 4 `:38-46`, step 2 `:27-30` (words unchanged) | ✅ kind check 7 of 7, dir refusal 3 of 3, AC-1.6/1.7 16 of 16; order caveat F4 |
| AC-4.2 | diff `origin/main...HEAD` | ✅ only `SKILL.md` and the spec set; constitution diff 0 lines; `.spark/.guard` untracked; `*.py` 0; `plugin.json` untouched (D6) |
| NFR-1 | `wc -l` 75 | ✅ |
| NFR-2 | `evidence.md` labels *development* | ✅ no guarantee language (grep clean); QA owns the figures |
| NFR-3 | frontmatter `:1-9` identical; argument and file name unchanged | ✅ |
| NFR-4 | T9 `shasum` re-checked | ✅; `aspark-graph` clause `not-verified-live` (user ruling Q3) |
| NFR-5 | rules at steps 2/7/8; I/O `:18-20`, refusals, Never `:73-75` | ✅; F2 notes one dropped sub-rule |
| NFR-6 | no new tracked files beyond the spec set; `claude plugin validate .` passes (1 pre-existing warning) | ✅ |
| NFR-7 | p1-p3 re-derived | ✅ 0 mentions, porcelain empty |

## 5. What Was Checked

- [x] Correctness: each rewording read against its ACs, the parent spec, the template and the README. No stale quote of the old wording anywhere: `grep` for "say so once", "run or offer nothing" and "no check was run" finds nothing outside `.spark/`
- [x] Non-functional: NFR-1 to NFR-7 and constitution §3-§6; library lens (surface unchanged, a patch bump fits a behaviour fix, contract restated)
- [x] Error handling: refusals still write nothing (T8 x-notes/x-empty, T9 old: porcelain empty)
- [x] Security: the hostile goal (m-r5, t4c) was recorded as text: no approval, no commit, Status `draft`
- [x] Tests: `claude plugin validate .` passes; the dev sessions were re-scored from raw output (F1, F4, F6)
- [x] Readability: the three edits are short and sit at the step where the agent acts

## 6. Verdict

Pass. The product change does what the plan says: each of B6, B4 and B8 is reworded at step 2, 7 and 8, where the agent acts. No step moved. The frontmatter is unchanged. The file is 75 lines and no line is wider than 90 characters. The word-level diff shows only the three edits and the two D3 trims, and the B1 and B3 wording changed in whitespace only. The fence is clean. I re-derived the evidence from the raw sessions and it holds, with one correction: AC-1.2 is **4 of 6**, not 5 of 6, because s-d2's "one other instance… at `approved` or `running`? No." mentions a `draft` instance that step 2 forbids mentioning (F1). That AC has no floor, so this is a recorded Minor, not a stop. However, s-h1 misreads "clash or not", and QA should watch for that. The trim of "run or offer nothing" dropped a sub-rule the evidence says is still stated elsewhere (F2). The happy path never emits the kind check before the Write, 0 of 37 (F4); this is most likely pre-existing, and QA must settle that. **T11 ruling:** being `blocked` until `/demo-day` is the approved design (D5, spec A4), and the plan's dependency column shows it. It is accepted for this gate and is not a finding. The condition is binding: T11 is done in `/increment` fix-mode after `qa.md` exists, with rate figures only, and is re-checked before `/go-live`. If T11 is not done, `/go-live` must not proceed. Reviewer edits, all to artifacts and not to product: F5 (plan Handoff `Open`, stale SHAs, "steps 1-8", evidence line ranges).

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place — never duplicate it as a second gate.*

- [x] No open Blocker findings
- [x] No open Major findings (or explicitly waived by the user, with reason recorded here). F4 becomes Major only if QA shows a regression against `origin/main`
- [x] Every Must AC traces to implementing code; no constitution non-negotiable violated
- [x] All plan deviations documented and accepted. T11 `blocked` is accepted under the §6 condition. The step-1 re-wrap is now documented (F5). The F2 trim conforms to D3, and its premise stays open as a Minor
- [x] Test suite runs green (no suite exists, constitution §4: `claude plugin validate .` passes and the dev sessions were re-derived)
- [x] Line budget respected: Ist 100 / Soll ~150 (excluding HTML comments) — self-reported, no linter checks this
- [x] Status set to `passed`
