# Review Report: adversarial-gates-standard

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | `c829176..HEAD` (`0c9144a` original, `a3d1d1c` round-1 fixes), `.spark/adversarial-gates-standard/plan.md`, parent `.spark/adversarial-gates/{spec,plan}.md` |
| **Status** | `passed` |
| **Round** | 2 |
| **Date** | 2026-10-27 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Passed. F1–F3 (Major) and F5–F7 are fixed and were confirmed from source. Two stale-count and wording defects that the fixes introduced were fixed by the reviewer (F9, F10). AC-2.1–2.4, AC-2.8, the NFR-2 live re-run and NFR-6 `validate` stay `⚠ not-verified-live`. They are disclosed, not waived.
- **Open:** `none` — Blockers: none; Majors: none (Minors/Nits: see §3)
- **Binding ruling:** §6 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

- **Reviewed:** the round-1 fix commit `a3d1d1c` in full (`standards/anti-generosity.md`, `plan.md`, `evidence.md`, `CONTRIBUTING.md`, `README.md`, `ROADMAP.md`, `docs/status.md`). Also re-checked against the cumulative `c829176..HEAD`: both skills' dispatch lines (`peer-review/SKILL.md:56-58,63-64`, `demo-day/SKILL.md:89-91,101-102`), parent spec AC-2.1–2.9, parent plan T11–T15, and corpus E3/E4 (`.spark/adversarial-gates/evidence.md:89,91`).
- **Evaluation mode:** static reading plus structural commands (`wc -l`, `grep -rn "standards/"`, `git diff --numstat/--stat`, JSON parse). **No live session was possible**: `which claude` returned nothing. Every behavioural item is degraded to `⚠ not-verified-live`. Anti-generosity standard applied. The caller did not pass it; the reviewer read it in full from the working tree (AC-2.8 note: this run was not dispatched through the skill).
- **Re-derived, not cited:** every F1–F8 fix, under condition (a) (verifying a fix). Also the activation grep, numstat, `wc -l`, the agents/templates diff and the E3/E4 entries, under condition (b) (AC-2.x verification). Branch staleness: the merge-base `c829176` is 5 behind `origin/main`, and the changed-file overlap (`comm -12`) is still empty.
- **Not reviewed:** live behaviour of either skill (no CLI). `file:line`s below refer to the 34-line file at HEAD plus the reviewer fixes.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T10 | ⚠️ | The grep baseline holds. The `/sprint-plan` silence session and the branch gate are now recorded deviations (`plan.md:21-22`). The silence session is `⚠ not-verified-live`. |
| T11 | ✅ | 34 lines, D7 frontmatter, rules 1–5. The scope bullet is present (`:11`, after F10). Labelling meets AC-2.9. Rule 6 is removed and the removal is recorded (`plan.md:22`). |
| T12 | ✅ | Unchanged since r1. Numstat is 5/1 per skill (cap 12). Literal path, no flag. |
| T13 | ⚠️ | Structural part PASS (grep hits only `peer-review:56`, `demo-day:89`). The status now reads `done (structural); live k=6 runs not performed` (`plan.md:15`), which is honest. The live k=6 runs are `⚠ not-verified-live`. |
| T14 | ⚠️ | Status `not-verified-live` (`plan.md:16`). No run was performed. Recorded as a deviation. |
| T15 | ✅ | CONTRIBUTING now names `standards/` (`CONTRIBUTING.md:105-107`). README states rule 3 (`README.md:133`). Inc 1 claims are still true. The line-count claims were stale (F9, fixed). The T10 re-run is a recorded deviation. |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Major | `standards/anti-generosity.md:14-34`; `evidence.md:24` | **AC-2.9 fails.** Only rule 1 cited the corpus and only rule 5 was labelled. r2: rule 1 cites E3, rule 2 cites E4 and E3. The tendency paragraph and rules 3, 4 and 5 are each labelled "not grounded in the corpus". `evidence.md:24` matches. | fixed r2 |
| F2 | Major | `.spark/adversarial-gates-standard/plan.md:15-16,21-22` | T13/T14 were marked `done` with their live DoD unperformed. r2: the statuses name what was not performed, and the DoD change is recorded under "Deviations". | fixed r2 |
| F3 | Major | `standards/anti-generosity.md` (former `:39-40`); `README.md:133` | Rule 6 was unplanned and undocumented, and the README omitted rule 3. r2: rule 6 is deleted and the deletion is recorded (`plan.md:22`). README `:133` now states the evaluation-mode rule. | fixed r2 |
| F4 | Major | `README.md:126`, `ROADMAP.md:44`, `docs/status.md:267-268` | Increment 1 claims went stale (AC-4.6). Reviewer fix r1 ("no skill gained a row"; the status points to Inc 2). Re-read r2: still true. | fixed r2 |
| F5 | Minor | `plan.md:21-22`; `CONTRIBUTING.md:105-107` | Three parent DoD items were dropped without a deviation. r2: all three are recorded, and the CONTRIBUTING line is added. | fixed r2 |
| F6 | Minor | `standards/anti-generosity.md:11` | The "never sets `approved` or waives" bullet was missing. r2: added. Its wording is refined by F10. | fixed r2 |
| F7 | Minor | `standards/anti-generosity.md:19,22` | Contradiction with the template's `⚠️ partial` and the "Result" column name. r2: "Overrides a template's `⚠️ partial`" and "Verdict/Result cell". | fixed r2 |
| F8 | Minor | `standards/anti-generosity.md:33-34` | Phrase count miscounted. Reviewer fix r1. Re-read r2: "six examples, the first two from the spec" is correct. | fixed r2 |
| F9 | Minor | `ROADMAP.md:45`, `docs/status.md:274`, `evidence.md:20` | The F3 fix shrank the file to 34 lines, but all three places still claimed "40 lines". `evidence.md:20` even printed `wc -l` = 40. Why it matters: a recorded command output that is false is the claim class constitution §8 bans. **Reviewer fix:** all three now say 34, matching `wc -l`. | fixed r2 |
| F10 | Minor | `standards/anti-generosity.md:11` | The F6 fix read "You never approve a gate". The Reviewer and QA Tester do set Status `passed` at their own gate (`templates/review-report.md:8,115`), so an agent could read this as forbidding that. Why it matters: a contradictory instruction makes gates stall (parent R6). **Reviewer fix:** "You never set `approved` or waive a finding", which is the T11 DoD's own wording. Still 34 lines. | fixed r2 |

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-2.1 | `standards/anti-generosity.md:14-15,31-34` | ⚠ not-verified-live — the text is present; no run shows the banned phrases staying out of a verdict |
| AC-2.2 | `:17-24` | ⚠ not-verified-live — the text is present (binary, exact cell); no graded fixture |
| AC-2.3 | `:25-27` | ⚠ not-verified-live — the text is present; no report observed stating its mode |
| AC-2.4 | `:28-30` | ⚠ not-verified-live — the text is present; no run observed keeping a finding |
| AC-2.5 | one file, 34 lines; `peer-review/SKILL.md:56-58`, `demo-day/SKILL.md:89-91`; empty `agents/` diff | PASS — `wc -l` 34; `git diff --stat c829176 -- agents` empty |
| AC-2.6 | parent `plan.md:115`, `evidence.md:8-14` | PASS — grep re-run r2, 2 hits only (`standards/`) |
| AC-2.7 | — | PASS — `git diff --stat c829176 -- templates` empty |
| AC-2.8 | `peer-review/SKILL.md:56,63-64`, `demo-day/SKILL.md:89,101-102` | ⚠ not-verified-live — unconditional dispatch text PASS (static); "the report names it" never observed |
| AC-2.9 | `:14-34` | PASS r2 — every rule is cited (E3/E4) or labelled "not grounded in the corpus" (F1) |
| AC-4.6 | `README.md:126,130-135`, `ROADMAP.md:44-45` | PASS r1 after the F4 fix — re-read r2, unchanged |
| NFR-1 | numstat 5/1 per skill; 34-line file | PASS |
| NFR-2 | `README.md:135`, `repo-layout.md:10` | PASS for the docs. The live silence re-run is `⚠ not-verified-live` (recorded deviation) |
| NFR-3 | hunks at `:53`/`:86` only; frontmatter untouched | PASS |
| NFR-4 | `plugin.json:4` 0.15.0; templates unchanged; `README.md:133` | PASS r2 — rule 6 is removed, so no undisclosed verdict change remains (F3) |
| NFR-5 | `README.md:132-134` | PASS r2 — all five rules are summarised (F3) |
| NFR-6 | `git ls-files '*.py'` empty; `plugin.json` parses | ⚠ not-verified-live — `claude plugin validate` not run (no CLI) |
| NFR-7 | no ID renumbered | PASS |
| NFR-8 | `README.md:134`, `ROADMAP.md:45` | PASS — "effect unmeasured", no claim that it fires |

## 5. What Was Checked

- [x] Correctness: each AC-2.x/4.6 item traced; behavioural items degraded to `⚠ not-verified-live`
- [x] Non-functional: NFR-1–8 and the `library` lens (public surface, compatibility, contract) checked
- [x] Error handling: N/A, no executable code. The degraded path (no CLI) is recorded honestly in plan and evidence
- [x] Security: no secrets and no executable code added; NFR-9 N/A per spec
- [ ] Tests: no test suite exists (no `*.py`, no CI). The live dry runs that stand in for tests were not performed
- [x] Readability: the standard is concise; F10 was its last contradiction

## 6. Verdict

Passed. This is a structural pass with disclosed gaps. Every round-1 Major is fixed and was confirmed from source. AC-2.9 now holds: every rule cites E3/E4 or is labelled not grounded. T13 and T14 no longer claim `done` for runs that never happened. The unplanned rule 6 is gone, and its removal and the three dropped parent DoD items are recorded as deviations. The README states every shipped rule. The fixes introduced two defects, and I fixed both: stale "40 lines" claims in three files, one of them a false recorded `wc -l` (F9), and a scope sentence that could stop a reviewer from setting its own `passed` (F10). Disclosed, not waived: AC-2.1–2.4 and AC-2.8 (US-2, a Should story), the NFR-2 live silence re-run and NFR-6 `claude plugin validate` are `⚠ not-verified-live`. No `claude` CLI existed, so no observed rate backs the claim that the standard changes any verdict. No Must acceptance criterion is unverified; AC-4.6 passes on static evidence. Only the user can accept shipping with the live items unmeasured, and this report does not do that on their behalf. `/demo-day` should run them where a CLI exists. The reviewer's r2 edits (`standards/anti-generosity.md:11`, `ROADMAP.md:45`, `docs/status.md:274`, `evidence.md:20`) are uncommitted.

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place — never duplicate it as a second gate.*

- [x] No open Blocker findings
- [x] No open Major findings (F1–F4 fixed r2; none waived)
- [x] Every Must AC traces to implementing code; no constitution non-negotiable violated (AC-4.6 PASS; §8 respected: no unperformed step is claimed as performed after F2/F9)
- [x] All plan deviations documented and accepted (`plan.md:21-22`; the reviewer accepts them as an honest recording of what was not performed, not as verification; the unmeasured live items remain the user's call)
- [x] Test suite runs green (N/A: no suite exists. Live dry runs and `validate` are `⚠ not-verified-live`, disclosed in §4/§6, not waived)
- [x] Line budget respected: Ist 101 / Soll ~150 (excluding HTML comments)
- [x] Status set to `passed`
