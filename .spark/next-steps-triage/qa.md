# QA Report: next-steps-triage

| | |
|---|---|
| **Phase** | Review (hands-on) |
| **Owner** | QA Tester (`/demo-day`) |
| **Input** | Installed plugin (no browser surface, constitution §8), `.spark/next-steps-triage/spec.md`, issue #110 ACs as mapped in `plan.md` §1/§3 |
| **Status** | `changes-requested` |
| **Round** | 1 |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** No, not yet: the AC-1 fixture (F1) returned `foundation` where the skill's own rule and plan expect `existing #1`; AC-5/AC-6 are partial by waiver.
- **Open:** `1 open` — Blockers: none; Majors: `B1` (Minors: B2)
- **Binding ruling:** §5 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/demo-day` and proceed — don't stop on it.

## 1. Test Environment

- **App URL:** N/A (no browser-observable surface). Method: `.spark/constitution.md` §8 substitute method: a performed step is a real ceremony/agent invocation or an observed command output. `git ls-files '*.py'` returned 0 (verified live).
- **Browser / viewport(s):** N/A.
- **Test data:** 4 fixture briefs below, 1 `aspark:product-owner` session each in propose mode (agent told not to read files; brief only; caller asked for the Factory JSON block). Branch `factory/issue-110`. No `gh`/remote, so no live `/next-steps` run. The briefs were written by me from plan §3, with small additions (noted per row). The Factory JSON key schema (SKILL.md step 4) was not in the brief, because that step belongs to the skill, not the agent.

## 2. Acceptance Criteria Verification

Raw first line and JSON are quoted from each session; full text is in the session transcript.

| Spec ID | Steps performed | Expected | Observed | Result |
|---|---|---|---|---|
| AC-1 | F1 brief: greenfield "Tasklet", 3 files, 1 commit, open #1 "Set up test harness", open #2 "Add login", both ready; gap list = CI, layout (constitution) | Plan §3: `existing #1`, foundation before #2 (SKILL Rules: "Reuse an open foundation ticket if one exists (`existing #N`)") | First line `**Type: foundation**`. Foundation slice (layout, lint, CI, harness) that "should absorb #1 ... Close #1 when the slice ships"; #1 listed only as alternative `existing #1`; #2 deferred. JSON: `"recommendation_type": "foundation"`, `"existing_issue": null`, `"absorbs_issues": [1]`. Foundation-first holds; reuse-of-open-ticket clause does not. F1b (no #1) not run. | ❌ fail (B1) |
| AC-2 | F2 brief: building, #5 "Export report" (blocked by #4), #4 open and ready | `existing #4`, JSON `existing_issue: 4` | First line `existing #4`; #5 given as alternative "blocked by #4 ... only after #4 ships". JSON `"type": "existing"`, `"existing_issue": 4` (plus invented keys `issue`, `rationale`; agent said it had no schema). | ✅ pass (n=1) |
| AC-3 | F3 brief: CI covered by closed #8; ROADMAP "audit log" has no issue; open #12 | "audit log" listed as gap with source; CI not a gap; pick = gap has no `existing_issue` | First line `gap`; audit log, source `ROADMAP.md Next`; "The CI gap is closed by #8". JSON `"type": "gap"`, `"source": "ROADMAP.md Next"`, but `"existing_issue": null` present instead of omitted (see B2). | ✅ pass (n=1), B2 |
| AC-4 | F4 brief: open #7 with merged PR #20 "Closes #7"; open #11 | #7 under "done, close it", never recommended, nothing closed | First line `existing #11`; "#7 ... is already done ... Close it as housekeeping. It is not a candidate". JSON has `close_list: [{issue: 7}]`; #7 not the pick. Nothing was closed (agent has no tools). | ✅ pass (n=1) |
| AC-5 | Read only: `skills/next-steps/SKILL.md:72-79`, `agents/product-owner.md` step 0. No `gh`, no no-ticket session run (plan T2 skipped) | One "Could not see the tickets" line in the closing report, triage/gap skipped | Text present at `SKILL.md:74-79`; agent told not to mention it. Not performed live. | ⚠️ partial / not-run (user waiver F3) |
| AC-6 | F2 and F3 JSON (above); `SKILL.md:117-125`, `docs/workflow.md:42` read. Factory `proposals.py` is in another repo | `existing_issue` integer only when `type` is `existing`; Factory reuses the issue | Field defined in SKILL.md and docs; F2 emitted integer 4; F1/F3 emitted `null` rather than omit. Skill-level JSON emission and Factory parsing not run. | ⚠️ partial (Factory side out of repo) |

NFRs: the spec defines none. Lenses: none active for QA.

## 3. Exploratory Findings

| # | Severity | Steps to reproduce | Expected vs. observed | Status |
|---|---|---|---|---|
| B1 | Major | Hand the PO agent the F1 brief (greenfield, open ready "Set up test harness" #1, open #2, gap list CI/layout) in propose mode | Expected `existing #1` (SKILL.md Rules "Reuse an open foundation ticket if one exists"; `product-owner.md` step 0 "reuse an open foundation ticket"). Observed `foundation`, which would create a new slice and "absorb" #1, so the Factory could file a duplicate of #1. 1 session, not repeated; may be variance, but not a waived outcome (the waiver covers n=1, not a miss). My brief added a CI/layout gap list that plan F1 lacks, which may have pulled the agent toward `foundation`. | open |
| B2 | Minor | F3 brief, read JSON | Expected key `existing_issue` omitted for `gap`. Observed `"existing_issue": null`. The agent had no schema in the brief; the skill (not the agent) owns the JSON. A Factory treating null as absent is fine (plan T6). Not reproduced through the skill. | open |

## 4. Console & Network

N/A, no browser. No commands failed; no remote or `gh` calls made. The sessions made 0 tool calls.

## 5. Verdict

I would not demo this yet. Three of four fixtures behave as intended (AC-2, AC-3, AC-4, 1 session each, anecdotal). F1 returned `foundation` in place of the reuse-ticket outcome that the skill rule and plan fixture require (B1, Major). AC-5 and AC-6 are partial and honestly not run (no `gh`, Factory in another repo); they are user-waived (review F3), as is the owed `claude plugin validate .` (review F4), and are not counted as failures. Needed: rerun F1 (ideally 3 sessions per plan §4, plus F1b) and either tighten the wording so an open foundation ticket is reused, or record the observed rate; then re-test.

---

## ✅ QA GATE

- [ ] Every Must-story acceptance criterion verified and passed (AC-1 failed; AC-5/AC-6 partial by waiver)
- [x] Every browser-observable NFR verified and passed (none defined)
- [ ] No open Blocker or Major bugs (B1 open)
- [x] Browser console free of errors on the tested flows (N/A, no browser)
- [x] Tested on all agreed viewports (N/A)
- [x] Line budget respected: Ist ~80 / Soll ~130 (excluding HTML comments)
- [ ] Status set to `passed` (`changes-requested`)
