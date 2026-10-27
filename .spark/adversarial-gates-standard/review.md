# Review Report: adversarial-gates-standard

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | `HEAD~1..HEAD` (`0c9144a`), `.spark/adversarial-gates-standard/plan.md`, parent `.spark/adversarial-gates/{spec,plan}.md` |
| **Status** | `changes-requested` |
| **Round** | 1 |
| **Date** | 2026-10-27 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Changes requested. The structure is correct, but AC-2.9 fails, T13/T14 are marked `done` with nothing performed, and rule 6 was neither planned nor documented. AC-2.1–2.4 and 2.8 are `⚠ not-verified-live`.
- **Open:** `6 open` — Blockers: none; Majors: `F1`, `F2`, `F3` (Minors/Nits: see §3)
- **Binding ruling:** §6 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

- **Reviewed:** all 11 files in `0c9144a`, plus surrounding context: both skills' step 2 and present step, `templates/review-report.md`, `templates/qa-report.md`, corpus E3/E4 and their sources (`.spark/project-kickoff/review.md:57`, `.spark/graph-mcp-verification/review.md:80`), constitution §8, and the parent plan's D7/D8 and T10–T15.
- **Evaluation mode:** static reading plus structural commands (`git diff --numstat`, `grep`, `wc -l`, JSON parse). **No live session was possible** (`which claude` returned not found). Every behavioural item is therefore degraded to `⚠ not-verified-live`. The standard under review was not passed to this reviewer. The strict verdicts here follow the caller's instruction. Lenses: `library` only.
- **Branch:** merge-base `c829176` is 5 commits behind `origin/main` (`11b725b`). The changed-file sets do not overlap (`comm -12` is empty). This is noted, not a finding (CLAUDE.md staleness habit).
- **Re-derived, not cited:** the T13 grep, the numstat, `wc -l` and the E3/E4 quotes. The triggering condition is (b): each one verifies an AC-2.x item.
- **Not reviewed:** live behaviour of either skill, because no CLI was available. Also not reviewed: uncommitted edits to `standards/anti-generosity.md` (now 34 lines, rule 6 removed, rules relabelled) and to `README.md:133`. Someone other than this reviewer made them while this report was being written. They appear to address F1, F3, F6, F7 and F8, but no fix was claimed through `/increment`, so every status below reflects `HEAD` plus the reviewer fixes only. Verify them in round 2. `file:line`s in §3/§4 refer to the 40-line committed file.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T10 | ⚠️ | Grep baseline holds. The parent T10 also required one recorded `/sprint-plan` session, to be re-run in T15 as the NFR-2 silence proof. Evidence calls it "a structural dry run, not a session". The step was dropped without a recorded deviation (F5). |
| T11 | ⚠️ | 40 lines, D7 frontmatter, and rules 1–5 match the DoD. Two deviations: the "never sets `approved` or waives" bullet is absent (F6), and an unplanned rule 6 was added (F3). AC-2.9 labelling is incomplete (F1). |
| T12 | ✅ | Step 2 gains 3 lines (cap 4) and the present step 1 net line; numstat is 5/1 per skill (cap 12). Literal `${CLAUDE_PLUGIN_ROOT}` path, no flag. |
| T13 | ❌ | Activation grep: PASS (hits only `peer-review/SKILL.md:56,64` and `demo-day/SKILL.md:89,102`). The k=6 dispatch runs per skill were not run, yet the status says `done` (F2). |
| T14 | ❌ | No run was performed (k=0 of 3 per skill), yet the status says `done` (F2). |
| T15 | ⚠️ | README, ROADMAP, status and repo-layout are updated, and the version is 0.15.0. Three gaps: `CONTRIBUTING.md` does not name `standards/` (F5), the T10 re-run is missing (F5), and Increment 1 claims went stale (F4, fixed). |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Major | `standards/anti-generosity.md:14-16,22-34,39-40`; `evidence.md:24` | **AC-2.9 fails.** Only rule 1 cites E3/E4, and only rule 5 carries "not grounded". The tendency paragraph and rules 2, 3, 4 and 6 do neither. Rule 2 visibly derives from E3 but does not cite it. `evidence.md:24` reports AC-2.9 as met. Why it matters: the AC exists so that ungrounded rules are labelled as such. Fix: cite E3/E4 on rule 2 and add "not grounded in the corpus" to the others, within 40 lines. Correct evidence.md:24. | open |
| F2 | Major | `.spark/adversarial-gates-standard/plan.md:13-14` | T13 and T14 are `done`, but their DoD is unperformed: the k=6 dispatch rate and the k=3 verdict rates were never measured. The re-cut also says it "changes no decision" while it rewrites both DoDs. Why it matters: a task marked `done` on an unperformed step is the exact rounding-up that this feature bans (constitution §8). It also hides that AC-2.1–2.4 and 2.8 are unverified. Fix (Engineering Manager): set T13/T14 to a non-done status such as `blocked — no CLI`, and record the DoD change as a deviation. | open |
| F3 | Major | `standards/anti-generosity.md:39-40` | Rule 6 makes a `passed` report impossible with any `⚠ not-verified-live` on a Must item unless the user waives it in writing. The spec ACs do not ask for this, T11's DoD does not list it, and the README (`README.md:133`) does not mention it. The README also omits rule 3 (stating the evaluation mode). Why it matters: this is the largest verdict-behaviour change in the file, and installed projects are not told about it (NFR-4, NFR-5). Fix (Engineering Manager): either plan it and add it to the README and ROADMAP, or delete rule 6. | open |
| F4 | Major | `README.md:126`, `ROADMAP.md:44`, `docs/status.md:267-268` | Increment 1 claims became false at this head. The docs said "every skill is unchanged", "no skill changed" and "every SKILL.md is byte-identical", and `status.md:268` still said the standard "is planned, not built". This violates AC-4.6 ("Increment 1's claims remain true") and AC-4.1. **Reviewer fix:** the wording now reads "no skill gained a row", and status.md:268 points to the Increment 2 section. | fixed |
| F5 | Minor | `plan.md:12,15`; `CONTRIBUTING.md` | The re-cut dropped three parent DoD items without recording a deviation: the T10 `/sprint-plan` silence session and its T15 re-run (the NFR-2 live proof), the T10 fresh-branch gate check, and naming `standards/` in CONTRIBUTING. Fix: record them as deviations or do them. CONTRIBUTING needs one line. | open |
| F6 | Minor | `standards/anti-generosity.md:9-12` | T11's DoD bullet "never sets `approved` or waives" is missing. Rule 6 mentions waivers only as the user's act. Why it matters: spec §6 puts self-approval out of scope, and the agent is not told this explicitly. Fix: add a half-line to the scope sentence. | open |
| F7 | Minor | `templates/review-report.md:85` vs `standards/anti-generosity.md:20,24-25` | The template, which is protected (AC-2.7), offers `⚠️ partial` as a verdict. The standard bans "partially" and requires the exact `⚠ not-verified-live`, but never says it overrides the template. It also says "Result cell", while the review template's column is "Verdict". Why it matters: the agent receives two contradictory instructions (parent plan R6). Fix: one clause in rule 2 ("this overrides a template's `⚠️ partial`; applies to any verdict/result cell"). | open |
| F8 | Minor | `standards/anti-generosity.md:37-38` | The text said "these five are examples chosen from the spec", but the list has six phrases and only two come from the spec (AC-2.1). **Reviewer fix:** it now reads "these six are examples (the first two from the spec)". Still 40 lines. | fixed |

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-2.1 | `standards/anti-generosity.md:14-16,35-38` | ⚠ not-verified-live — the text is present; no run shows banned phrases staying out of a verdict (plan line 133 classes this AC as live) |
| AC-2.2 | `:18-28` | ⚠ not-verified-live — the text is present (binary, exact cell); no graded fixture |
| AC-2.3 | `:29-31` | ⚠ not-verified-live — the text is present; no report observed stating its evaluation mode |
| AC-2.4 | `:32-34` | ⚠ not-verified-live — the text is present; no run observed keeping a finding |
| AC-2.5 | one file, 40 lines; `peer-review/SKILL.md:56-58`, `demo-day/SKILL.md:89-91`; empty `agents/` diff | PASS — `wc -l` 40; `git diff --stat -- agents` empty |
| AC-2.6 | parent `plan.md:115`, `evidence.md:7-13` | PASS — both phases have a dispatch row and an activation row; grep re-run with 4 hits only |
| AC-2.7 | — | PASS — `git diff --stat -- templates` empty |
| AC-2.8 | `peer-review/SKILL.md:56,63-64`, `demo-day/SKILL.md:89,101-102` | ⚠ not-verified-live — the unconditional dispatch text is PASS (static); "the run's report names it" was never observed |
| AC-2.9 | `:14-40` | FAIL — F1 |
| AC-4.6 | `README.md:130-135`, `ROADMAP.md:45` | PASS r1 after the F4 fix — always active, behaviour change and unmeasured are all stated; Increment 1 claims are true again |
| NFR-1 | numstat 5/1 per skill; 40-line file | PASS |
| NFR-2 | `README.md:135`, `repo-layout.md:10` | PASS for the docs naming where it is silent. The live silence run is `⚠ not-verified-live` (F5) |
| NFR-3 | hunks at `:53`/`:86` only; frontmatter untouched | PASS |
| NFR-4 | `plugin.json:4` 0.14.3→0.15.0; templates unchanged; `README.md:133` | PASS for the structure. The recorded dry run is `⚠ not-verified-live`. Rule 6 is undisclosed (F3) |
| NFR-5 | `README.md:132-134` | FAIL — rule 3 (evaluation mode) and rule 6 are not documented (F3) |
| NFR-6 | `git ls-files '*.py'` empty; `plugin.json` parses as JSON | ⚠ not-verified-live — `claude plugin validate` was not run (no CLI) |
| NFR-7 | no ID renumbered | PASS |
| NFR-8 | `README.md:134`, `ROADMAP.md:45` | PASS — "effect unmeasured", no claim that the standard fires |

## 5. What Was Checked

- [x] Correctness: each AC-2.x/4.6 item traced; behavioural items degraded to `⚠ not-verified-live`
- [x] Non-functional: NFR-1–8 and the `library` lens (public surface, compatibility, contract) checked
- [x] Error handling: N/A, no executable code. The degraded path (no CLI) is checked under F2
- [x] Security: no secrets and no executable code added; NFR-9 N/A per spec
- [ ] Tests: no test suite exists (no `*.py`, no CI). The live dry runs that stand in for tests were not performed
- [x] Readability: the standard is concise; F7 is the one contradiction

## 6. Verdict

Changes requested. The delivery shape is correct and structurally verified: one 40-line file, dispatched unconditionally by literal path in both claimed skills, within the line caps, with no agent, template or constitution change and a minor bump. The README states the always-on verdict change. But AC-2.9 fails (F1), because four of six rules and the tendency paragraph are neither cited nor labelled. The plan marks T13 and T14 `done` although none of their live runs happened (F2). The standard also adds a rule nobody planned or documented (rule 6), and that rule makes an unverified Must item block `passed` (F3). That is the largest consumer-visible change in the file. Every behavioural criterion (AC-2.1–2.4, AC-2.8) and `claude plugin validate` remain `⚠ not-verified-live` because no `claude` CLI existed. This report does not count that as a pass. Under the feature's own rule 6 that result alone would keep the report from passing, although US-2 is a Should story and the user may waive it. I fixed the stale Increment 1 doc claims (F4) and the miscounted phrase list (F8) directly. The pre-existing defects these fixes touched were the doc wording and a count; no design question was involved. F1–F3 go back to `/increment` and the Engineering Manager.

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place — never duplicate it as a second gate.*

- [x] No open Blocker findings
- [ ] No open Major findings (F1, F2, F3 open; none waived)
- [x] Every Must AC traces to implementing code; no constitution non-negotiable violated (AC-4.6 PASS after F4; constitution §8 respected in this report; F2 is a plan-status overclaim, not a shipped claim)
- [ ] All plan deviations documented and accepted (F2, F3, F5, F6)
- [ ] Test suite runs green (none exists; live runs and `validate` are `⚠ not-verified-live`)
- [x] Line budget respected: Ist 100 / Soll ~150 (excluding HTML comments)
- [ ] Status set to `passed`
