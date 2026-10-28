# Review Report: next-steps-triage

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | The diff of `/increment` (`64767bc`, `7ece7f7`, fix pass `6b10800`), `.spark/next-steps-triage/plan.md` |
| **Status** | `changes-requested` |
| **Round** | 2 |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Changes requested: F1 is resolved by the user's ruling and F3 is user-waived, but F4 stays open: no `ROADMAP.md` row and no `claude plugin validate` result, which the constitution's testing bar requires.
- **Open:** `2 open` — Blockers: none; Majors: `F4` (F3 waived by the user); Minors: `F8`
- **Binding ruling:** §6 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

Reviewed (r2): fix pass `6b10800` (`skills/next-steps/SKILL.md`, `agents/product-owner.md`, `docs/status.md`, `.spark/next-steps-triage/{spec,plan}.md`) against round 1's findings, plus the full `64767bc..HEAD` diff on `factory/issue-110` (base `7bdd6aa` = `origin/main`, branch not stale). The user's F1 and F3 rulings were relayed by the `/peer-review` caller (AskUserQuestion in its conversation). I could not see that exchange; I record the rulings as relayed.

Not possible here, unchanged from r1: issue #110's AC text (no `gh`), fixture or live sessions and `claude plugin validate` (no `claude` CLI). Template: `/opt/aspark/templates/review-report.md` (in place, one state, no round section). No tool file was passed. Both manifests parse as JSON (`python3 -I`), which is not a substitute for validate.

Re-derived from source, not cited: F1's facts (condition a: verifying the ruling that resolves it): `constitution.md:192-195` still says "no warning, no mention", `:199-200` gives waivers to the user and requires recording in the artifact, and `SKILL.md:74-79,114-115` still emits the line. F3's waiver facts (condition a): n=1 and the unrun paths are disclosed at `evidence.md:3,12`, `docs/status.md:288-290` and `spec.md:19`. Each fix in §3 (condition a). Every AC (condition b).

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ⚠️ | D1 fixed (`SKILL.md:97-101`), D2 resolved by numbering the new PO step `0` so step 6 stays the thin-brief rule (`product-owner.md:181`, documented at `evidence.md:16`), D3 (`product-owner.md:147`), D4 (`SKILL.md:77-79`). There are no per-AC `file:line` rows in `evidence.md`. Q1/Q2 were agent-ruled at r1 (F1); r2: Q1 now user-ruled, recorded in `evidence.md`; Q2 chose the §6-conformant option. |
| T2 | ❌ | Not run (`evidence.md:12`). The plan said `done` (F2). |
| T3 | ⚠️ | 1 session per fixture, not 3. F1 as specified (open #1 → `existing #1`) and F1b were not run; the evidence's F1 row is a 0-issue variant. No briefs and no raw output quoted. Accepted by the user's F3 waiver. Sessions predate the r1 PO edits (F13). |
| T4 | ❌ | Not run (`evidence.md:12`). The plan said `done` (F2). |
| T5 | ⚠️ | No per-site rows in `evidence.md`. I checked all 9 sites myself (`spark/SKILL.md:32,34,125`, `charter/SKILL.md:34`, `backlog/SKILL.md:35`, `product-owner.md:7`, `README.md:76`, `docs/workflow.md:24`, `campaigns/README.md:72`, `field_report.yml:36`), and every one is still correct. |
| T6 | ✅ | `docs/workflow.md:36-45` uses the same key names as `SKILL.md:119-124` and states the integer-only/`existing`-only rule, Factory-side parsing, and that there is no handshake. |
| T7 | ⚠️ | r2: `docs/status.md:288-290` added and matches `evidence.md`; README best-effort caveat added by me. Still no `ROADMAP.md` row and no validate output (F4). |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Blocker | `skills/next-steps/SKILL.md:73-79,113-114`; `agents/product-owner.md:158`; `.spark/next-steps-triage/evidence.md:14` | AC-5's "Could not see the tickets" line contradicts `constitution.md:192-195` ("degrades to silence … no warning, no mention"). The plan itself called this a non-negotiable break that needs the **user's** ruling (`plan.md:73`). The ruling was made by the agent ("autonomous, plan-time"), and `constitution.md:198-199` reserves waivers for the user. There is no `/charter` amendment. The shipped prompt also cites that ruling ("ruled at plan time") to consumers who cannot see it. **Why it matters:** an agent waived a non-negotiable. **Fix:** the user rules, either by dropping the line (silent fallback) or with a recorded `/charter` amendment that scopes §6 to gates or exempts `/next-steps`. Then remove "ruled at plan time" from the prompt. **r2:** the user ruled (relayed by the caller): keep the line, waiving §6 for it, reason "issue #110 asks for it". That is the plan's own recorded-waiver option (`plan.md:73`), so the defect (an agent ruling) is resolved, not waived; the ruling is now recorded at `evidence.md:16`. "ruled at plan time" is gone from `SKILL.md`. | fixed r2 |
| F2 | Major | `.spark/next-steps-triage/plan.md:51-57` | All seven tasks were marked `done`, but T2 and T4 never ran, T3 ran 1 of 3 sessions per fixture, T5 has no evidence rows, T7 is missing two of its four deliverables and T1 lacks its evidence rows. The PLAN GATE and Handoff (`:14`) contradicted §3. **Why it matters:** downstream gates read `done` as verified. **Fix (applied):** statuses are now `skipped`/`partial` with a reason, and the Handoff Open line matches. | fixed r1 |
| F3 | Major | `.spark/next-steps-triage/evidence.md:3-12` | The Must-AC verification falls short of `constitution.md:151-153` (dogfood every phase touched, negative case first). There was no negative case (T2), AC-5 and AC-6 were checked by reading only (`§8`: reasoning is not a performed step), the F1 reuse path was not run, and every rate is n=1 with no fixture brief, first line or JSON quoted, so no one can check the 1/1. **Why it matters:** none of AC-1..AC-6 is shown to hold with checkable evidence. **Fix:** run T2 and T3 as planned, quoting briefs and raw output, or the user waives the reduced set with a recorded reason. The limitation is disclosed in `evidence.md`, but disclosure is not resolution. **r2:** user-waived (relayed), reason: reduced set disclosed. Facts re-derived: still true at `evidence.md:3,12`, `docs/status.md:290`, `spec.md:19`; no fix pass changed them. Not named in the waiver: unquoted briefs/output and the unrun F1 ticket-reuse path, both visible in `evidence.md:7` to whoever waived. | waived r2 |
| F4 | Major | `README.md:106`; `docs/status.md`; `ROADMAP.md`; `.spark/next-steps-triage/evidence.md` | Docs are not in step (`constitution.md:157-159`, plan T7). The README row states ticket triage as a capability with no best-effort caveat. `docs/status.md` has no section with the observed rates and `ROADMAP.md` has no row. `claude plugin validate` was never run or recorded (`constitution.md:151`). **Why it matters:** the README asserts current state, and per CLAUDE.md, prompt material documented without its rate reads as a guarantee. **Fix:** add the status section (n=1, best effort, AC-5/AC-6 unexercised), the ROADMAP row, "best effort" in the README row, and validate output in `evidence.md`. **r2:** `docs/status.md:288-290` done and accurate; I added the README caveat (`README.md:106`). Still open: no `ROADMAP.md` row (note `ROADMAP.md:7-9` reserves Shipped for exercised work, so a `partial` label or a recorded deviation from plan T7 is needed), and `claude plugin validate .` never run or recorded. | open |
| F5 | Minor | `skills/next-steps/SKILL.md:71-72` | `gh pr list --state all` had no `--limit`, so `gh` defaults to 30. An open issue whose merged PR is older than the last 30 PRs is never found as "done, close it" and could be recommended (AC-4). The plan's risk row (`plan.md:78`) specifies `--limit 200`. **Fix (applied):** added `--limit 200`. | fixed r1 |
| F6 | Minor | `skills/next-steps/SKILL.md:73-79` | The unreachable branch covers only "no tool works". If `gh issue list` succeeds but `gh pr list` fails, triage runs without PR data, so AC-4 is silently skipped and nothing is reported. **Fix:** when the PR list fails, skip the "done, close it" check and state in the brief that merged-PR linking was not checked. **r2:** `SKILL.md:114-116` now reports it in the closing report. | fixed r2 |
| F7 | Minor | `agents/product-owner.md:158`; `skills/next-steps/SKILL.md:113-114` | Both the PO ("say so once") and the skill's closing report ("say so once in this closing report") emit the unreachable note. The skill relays the PO's text, so the note can appear twice. **Fix:** keep the line only in the skill's closing report (CLAUDE.md "Put a required line where the output is written") and have the PO just propose. Depends on F1's ruling. **r2:** `product-owner.md:158` now defers to the caller; only `SKILL.md:114` says it. | fixed r2 |
| F8 | Minor | `agent.yaml:33` (protected; read-only here) | Q2 gates the JSON block on `agent.yaml`. That file's `tools.allow` has no `Bash(gh:*)`, so in a Factory session `gh` must be approved at the Leitstand and may time out and be denied. The tickets are then "unreachable" and `existing_issue` is never produced, which is AC-6's main purpose. **Fix:** route to aSPARK-Factory: confirm the proposal sessions allow read-only `gh`, or document the dependency in `docs/workflow.md:36-45`. `agent.yaml` is protected and was not touched. | open |
| F9 | Minor | `.spark/next-steps-triage/spec.md:19` | The spec's Disclosure says "No fixture sessions were run". Four were run (`evidence.md:7-10`). This is plan Q4, owned by the PO. **Fix:** the PO updates the Disclosure with the n=1 results. **r2:** `spec.md:19` now matches `evidence.md`. | fixed r2 |
| F10 | Nit | `agents/product-owner.md:135-140` | The propose-mode intro still lists the brief as shipped/in-flight/constitution only. Maturity, tickets and gaps appear only in step 0. **Fix:** add them to the intro sentence so the brief description matches what `SKILL.md:97-101` passes. **r2:** `product-owner.md:139`. | fixed r2 |
| F11 | Minor | `skills/next-steps/SKILL.md:75-76` | The F1 fix wrote "the project's own ticket #110" and "the constitution's silent-fallback rule" into shipped prompt text. In a consumer repo that points at *their* #110 and *their* constitution. **Fix (applied):** "one line by design, aSPARK issue #110: a deliberate exception to aSPARK's own silent-fallback rule, waived by its maintainer". | fixed r2 |
| F12 | Minor | `.spark/next-steps-triage/plan.md:14,51,57` | After the r1 fixes, the Handoff Open line still had Q1 agent-ruled and Q4 open, and T7's note still said there was no `docs/status.md` section. That is the same `done`-hides-state drift as F2. **Fix (applied):** the notes and Open line now reflect r2. | fixed r2 |
| F13 | Minor | `.spark/next-steps-triage/evidence.md` | The four sessions ran on the PO prompt from before `6b10800`, and evidence did not say so. The user's F1/F3 rulings were recorded nowhere, which `constitution.md:199-200` requires. **Fix (applied):** a prompt-version line and the user rulings were added (`evidence.md:14-16`). The F7/F10 edits do not touch any path a fixture exercised, so the F3 waiver's facts still hold. | fixed r2 |

## 4. Requirements Traceability

The AC text is in issue #110, which cannot be read here. AC-1 and AC-6 are inferred, per `plan.md:20`.

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1 greenfield → foundation | `SKILL.md:80-83,135-138`; `product-owner.md:147-151` | ⚠️ partial: text present, n=1 on a 0-issue variant, ticket-reuse path unrun |
| AC-2 blocked → blocker | `SKILL.md:84-89`; `product-owner.md:152-154` | ⚠️ partial: n=1, output not quoted |
| AC-3 gaps with source | `SKILL.md:90-94,111-113`; `product-owner.md:155-156` | ⚠️ partial: n=1, output not quoted |
| AC-4 merged PR → "done, close it" | `SKILL.md:84-86,140-141` | ⚠️ partial: n=1; F5, F6 fixed r2 |
| AC-5 tickets unreachable | `SKILL.md:73-79,113-114` | ⚠️ partial r2: §6 conflict waived by the user (F1); exercised by reading only, F3 waived |
| AC-6 Factory `existing_issue` | `SKILL.md:115-124`; `docs/workflow.md:36-45` | ⚠️ partial: field defined and documented; never emitted in a skill run; Factory side out of repo (F8) |

## 5. What Was Checked

- [x] Correctness: logic does what the acceptance criteria demand (by reading; AC-5's §6 exception is user-ruled, F1)
- [ ] Non-functional: applicable NFRs and constitution quality bars hold (no: §4 testing bar, `claude plugin validate` unrecorded, F4; §6 silence waived for one line by the user)
- [x] Error handling: failures are handled, not swallowed (F6 fixed r2)
- [x] Security: no injected input trusted, no secrets in code (`gh` read-only; never closes or files issues, `SKILL.md:140-141`)
- [ ] Tests: exist, are meaningful, and pass (no suite possible; reduced dogfood waived by the user, F3; validate missing, F4)
- [x] Readability: the next developer will understand this

## 6. Verdict

This still does not pass, but only for one reason. F1 is resolved: the user ruled to keep the "Could not see the tickets" line as a recorded exception to §6, which is the waiver path the plan itself offered, and I re-derived that the line, the constitution text and the ruling's grounds are what the ruling assumed. F3 is waived by the user; its stated facts (n=1, no-ticket and live paths unrun, disclosed in three places) still hold after the fix pass, which edited the PO prompt only on paths no fixture exercised (now disclosed, F13). Both rulings reached me through the caller, not first-hand; they are recorded in `evidence.md` as relayed, and the human at the gate should confirm them. The r1 fixes are real: F6, F7, F9 and F10 hold in source, and I fixed three new Minors (F11 consumer-facing "#110" wording, F12 stale plan notes, F13 unrecorded rulings). F4 stays an open, unwaived Major: there is no `ROADMAP.md` row, and `claude plugin validate .`, the constitution's one test bar, has never been run or recorded for this change. Every earlier feature on this repo recorded it. Path back: run validate where the CLI exists and paste the result into `evidence.md`, and add a `partial` ROADMAP row or record why T7 drops it. Or the user waives F4 with a reason. F8 (Factory `gh` permission) stays a routed Minor.

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place — never duplicate it as a second gate.*

- [x] No open Blocker findings (F1 fixed r2 by the user's ruling)
- [ ] No open Major findings (F3 waived r2; F4 open, not waived)
- [ ] Every Must AC traces to implementing code; no constitution non-negotiable violated (all trace; AC-5's §6 exception is user-waived and recorded, F1)
- [ ] All plan deviations documented and accepted (documented in `plan.md` §3; T2-T4 reductions accepted via the F3 waiver; T7 ROADMAP/validate gap not accepted, F4)
- [ ] Test suite runs green (no suite exists; `claude plugin validate` not runnable here and still not recorded, F4)
- [x] Line budget respected: Ist 96 / Soll ~150 (excluding HTML comments)
- [ ] Status set to `passed`
