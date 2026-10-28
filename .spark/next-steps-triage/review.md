# Review Report: next-steps-triage

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | The diff of `/increment` (`64767bc`, `7ece7f7`), `.spark/next-steps-triage/plan.md` |
| **Status** | `changes-requested` |
| **Round** | 1 |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Changes requested: the prompt change reads correctly, but AC-5 conflicts with a constitution non-negotiable that an agent waived, and AC-5/AC-6 plus the negative case were never exercised.
- **Open:** `8 open` — Blockers: `F1`; Majors: `F3`, `F4` (Minors/Nits: see §3)
- **Binding ruling:** §6 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

Reviewed: `64767bc..7ece7f7` on `factory/issue-110`, i.e. `skills/next-steps/SKILL.md`, `agents/product-owner.md`, `docs/workflow.md`, `README.md:106`, and `.spark/next-steps-triage/{spec,plan,evidence}.md`. Base: the branch sits on `origin/main` (`7bdd6aa`). Local `main` is stale at `ec48198`, so a diff against *its* merge-base also shows 14 files from merged PRs #108/#109 (`.spark/observability-lens-public-run/`, `.spark/website-lens-public-run/`, `ROADMAP.md`, `docs/status.md`). Those are already on `origin/main` and were not reviewed. The branch is not stale.

Not reviewed or not possible here: the AC text of issue #110 (not in the repo, and `gh` is not installed in this environment), so ACs are traced against the plan's own reading at `plan.md:20`. I could not run fixture sessions or `claude plugin validate` (no `claude` CLI). The Factory's `proposals.py` is in another repo. The caller's template path `/opt/aspark/templates/review.md` does not exist, so `templates/review-report.md` was used. No tool file was passed.

Re-derivation: every AC's verification was re-read from source (condition b, Must AC). The evidence's 1/1 rates could not be re-derived because no transcripts or fixture briefs exist (see F3).

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ⚠️ | D1 fixed (`SKILL.md:97-101`), D2 resolved by numbering the new PO step `0` so step 6 stays the thin-brief rule (`product-owner.md:181`, documented at `evidence.md:16`), D3 (`product-owner.md:147`), D4 (`SKILL.md:77-79`). There are no per-AC `file:line` rows in `evidence.md`. The agent ruled Q1/Q2 itself, but the plan required the user to (F1). |
| T2 | ❌ | Not run (`evidence.md:12`). The plan said `done` (F2). |
| T3 | ⚠️ | 1 session per fixture, not 3. F1 as specified (open #1 → `existing #1`) and F1b were not run; the evidence's F1 row is a 0-issue variant. No briefs and no raw output quoted (F3). |
| T4 | ❌ | Not run (`evidence.md:12`). The plan said `done` (F2). |
| T5 | ⚠️ | No per-site rows in `evidence.md`. I checked all 9 sites myself (`spark/SKILL.md:32,34,125`, `charter/SKILL.md:34`, `backlog/SKILL.md:35`, `product-owner.md:7`, `README.md:76`, `docs/workflow.md:24`, `campaigns/README.md:72`, `field_report.yml:36`), and every one is still correct. |
| T6 | ✅ | `docs/workflow.md:36-45` uses the same key names as `SKILL.md:119-124` and states the integer-only/`existing`-only rule, Factory-side parsing, and that there is no handshake. |
| T7 | ⚠️ | Only `README.md:106` changed. There is no `docs/status.md` section, no `ROADMAP.md` row, and no validate output (F4). |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Blocker | `skills/next-steps/SKILL.md:73-79,113-114`; `agents/product-owner.md:158`; `.spark/next-steps-triage/evidence.md:14` | AC-5's "Could not see the tickets" line contradicts `constitution.md:192-195` ("degrades to silence … no warning, no mention"). The plan itself called this a non-negotiable break that needs the **user's** ruling (`plan.md:73`). The ruling was made by the agent ("autonomous, plan-time"), and `constitution.md:198-199` reserves waivers for the user. There is no `/charter` amendment. The shipped prompt also cites that ruling ("ruled at plan time") to consumers who cannot see it. **Why it matters:** an agent waived a non-negotiable. **Fix:** the user rules, either by dropping the line (silent fallback) or with a recorded `/charter` amendment that scopes §6 to gates or exempts `/next-steps`. Then remove "ruled at plan time" from the prompt. | open |
| F2 | Major | `.spark/next-steps-triage/plan.md:51-57` | All seven tasks were marked `done`, but T2 and T4 never ran, T3 ran 1 of 3 sessions per fixture, T5 has no evidence rows, T7 is missing two of its four deliverables and T1 lacks its evidence rows. The PLAN GATE and Handoff (`:14`) contradicted §3. **Why it matters:** downstream gates read `done` as verified. **Fix (applied):** statuses are now `skipped`/`partial` with a reason, and the Handoff Open line matches. | fixed r1 |
| F3 | Major | `.spark/next-steps-triage/evidence.md:3-12` | The Must-AC verification falls short of `constitution.md:151-153` (dogfood every phase touched, negative case first). There was no negative case (T2), AC-5 and AC-6 were checked by reading only (`§8`: reasoning is not a performed step), the F1 reuse path was not run, and every rate is n=1 with no fixture brief, first line or JSON quoted, so no one can check the 1/1. **Why it matters:** none of AC-1..AC-6 is shown to hold with checkable evidence. **Fix:** run T2 and T3 as planned, quoting briefs and raw output, or the user waives the reduced set with a recorded reason. The limitation is disclosed in `evidence.md`, but disclosure is not resolution. | open |
| F4 | Major | `README.md:106`; `docs/status.md`; `ROADMAP.md`; `.spark/next-steps-triage/evidence.md` | Docs are not in step (`constitution.md:157-159`, plan T7). The README row states ticket triage as a capability with no best-effort caveat. `docs/status.md` has no section with the observed rates and `ROADMAP.md` has no row. `claude plugin validate` was never run or recorded (`constitution.md:151`). **Why it matters:** the README asserts current state, and per CLAUDE.md, prompt material documented without its rate reads as a guarantee. **Fix:** add the status section (n=1, best effort, AC-5/AC-6 unexercised), the ROADMAP row, "best effort" in the README row, and validate output in `evidence.md`. | open |
| F5 | Minor | `skills/next-steps/SKILL.md:71-72` | `gh pr list --state all` had no `--limit`, so `gh` defaults to 30. An open issue whose merged PR is older than the last 30 PRs is never found as "done, close it" and could be recommended (AC-4). The plan's risk row (`plan.md:78`) specifies `--limit 200`. **Fix (applied):** added `--limit 200`. | fixed r1 |
| F6 | Minor | `skills/next-steps/SKILL.md:73-79` | The unreachable branch covers only "no tool works". If `gh issue list` succeeds but `gh pr list` fails, triage runs without PR data, so AC-4 is silently skipped and nothing is reported. **Fix:** when the PR list fails, skip the "done, close it" check and state in the brief that merged-PR linking was not checked. | open |
| F7 | Minor | `agents/product-owner.md:158`; `skills/next-steps/SKILL.md:113-114` | Both the PO ("say so once") and the skill's closing report ("say so once in this closing report") emit the unreachable note. The skill relays the PO's text, so the note can appear twice. **Fix:** keep the line only in the skill's closing report (CLAUDE.md "Put a required line where the output is written") and have the PO just propose. Depends on F1's ruling. | open |
| F8 | Minor | `agent.yaml:33` (protected; read-only here) | Q2 gates the JSON block on `agent.yaml`. That file's `tools.allow` has no `Bash(gh:*)`, so in a Factory session `gh` must be approved at the Leitstand and may time out and be denied. The tickets are then "unreachable" and `existing_issue` is never produced, which is AC-6's main purpose. **Fix:** route to aSPARK-Factory: confirm the proposal sessions allow read-only `gh`, or document the dependency in `docs/workflow.md:36-45`. `agent.yaml` is protected and was not touched. | open |
| F9 | Minor | `.spark/next-steps-triage/spec.md:19` | The spec's Disclosure says "No fixture sessions were run". Four were run (`evidence.md:7-10`). This is plan Q4, owned by the PO. **Fix:** the PO updates the Disclosure with the n=1 results. | open |
| F10 | Nit | `agents/product-owner.md:135-140` | The propose-mode intro still lists the brief as shipped/in-flight/constitution only. Maturity, tickets and gaps appear only in step 0. **Fix:** add them to the intro sentence so the brief description matches what `SKILL.md:97-101` passes. | open |

## 4. Requirements Traceability

The AC text is in issue #110, which cannot be read here. AC-1 and AC-6 are inferred, per `plan.md:20`.

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1 greenfield → foundation | `SKILL.md:80-83,135-138`; `product-owner.md:147-151` | ⚠️ partial: text present, n=1 on a 0-issue variant, ticket-reuse path unrun |
| AC-2 blocked → blocker | `SKILL.md:84-89`; `product-owner.md:152-154` | ⚠️ partial: n=1, output not quoted |
| AC-3 gaps with source | `SKILL.md:90-94,111-113`; `product-owner.md:155-156` | ⚠️ partial: n=1, output not quoted |
| AC-4 merged PR → "done, close it" | `SKILL.md:84-86,140-141` | ⚠️ partial: n=1; F5 fixed, F6 open |
| AC-5 tickets unreachable | `SKILL.md:73-79,113-114` | ❌ conflicts with `constitution.md:192-195` (F1); never exercised (F3) |
| AC-6 Factory `existing_issue` | `SKILL.md:115-124`; `docs/workflow.md:36-45` | ⚠️ partial: field defined and documented; never emitted in a skill run; Factory side out of repo (F8) |

## 5. What Was Checked

- [x] Correctness: logic does what the acceptance criteria demand (by reading; AC-5 conflicts with the constitution)
- [ ] Non-functional: applicable NFRs and constitution quality bars hold (no: §4 testing and docs bars, §6 silence and waiver rules)
- [x] Error handling: failures are handled, not swallowed (partial-reachability gap F6)
- [x] Security: no injected input trusted, no secrets in code (`gh` read-only; never closes or files issues, `SKILL.md:140-141`)
- [ ] Tests: exist, are meaningful, and pass (no suite possible; dogfood not run as required, F3)
- [x] Readability: the next developer will understand this

## 6. Verdict

This does not pass. The prompt text is coherent: D1-D4 are really fixed, the six behaviours each have a clear home in `SKILL.md`, and the JSON contract in `docs/workflow.md` matches the skill's key names. But the review cannot sign off on three things. First, AC-5 as written breaks the constitution's degrade-to-silence non-negotiable, and an agent made the waiver, not the user. That stays a Blocker until the user drops the line or amends §6 through `/charter`. Second, the verification the constitution requires never happened: no negative case, AC-5 and AC-6 never exercised, and four unquoted n=1 sessions. Third, the docs claim the capability without its rate, and `claude plugin validate` was never run. The plan's `done` statuses hid all of this. I corrected them (F2) and added the missing `--limit 200` (F5). The spec's own Disclosure says this review is not to be written as `passed`, and with F1, F3 and F4 open it is not. Path back: the user rules on F1 (and on F3 if a reduced evidence set is acceptable, with the reason recorded here), then `/increment` fixes F4 and the Minors and re-runs the plan's T2/T3.

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place — never duplicate it as a second gate.*

- [ ] No open Blocker findings (F1 open)
- [ ] No open Major findings (F3, F4 open; none waived)
- [ ] Every Must AC traces to implementing code; no constitution non-negotiable violated (all trace; AC-5 violates §6, F1)
- [ ] All plan deviations documented and accepted (now documented in `plan.md` §3 and here; not accepted by the user)
- [ ] Test suite runs green (no suite exists; `claude plugin validate` not runnable here and not recorded, F4)
- [x] Line budget respected: Ist 93 / Soll ~150 (excluding HTML comments)
- [ ] Status set to `passed`
