# Review Report: backlog-seed

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | `git diff 4d6c4ac~1 4d6c4ac` (PR #82, issue #81), fix commit `120dfb1`, `.spark/backlog-seed/plan.md`, `evidence.md` |
| **Status** | `changes-requested` |
| **Round** | 2 |
| **Date** | 2026-10-25 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Changes requested. F1 is fixed. F2 (the constitution §4 dry run) is still only partly done: the negative case was read, not run, and steps 3-6 never ran.
- **Open:** `3 open`. Blockers: none. Majors: `F2` (Minors/Nits: see §3)
- **Binding ruling:** §6 Verdict and the gate checklist below. They are the only binding location; there is no other round to point to.
- **On conflict:** the numbered body below wins for everything except `Status`. Log the mismatch as a finding at the next `/peer-review` and proceed; don't stop on it.

## 1. Scope

Round 2 re-reviewed `120dfb1`: `skills/backlog/SKILL.md`, `skills/spark/SKILL.md:29-36`, `docs/status.md:117,286`, `docs/family.md:16`, `.spark/backlog-seed/plan.md`, `evidence.md`. Every open round-1 finding was re-derived from primary source (condition (a), verifying a fix). Also checked: `agents/product-owner.md:192-206`, `.spark/constitution.md:149-154,329`, `ROADMAP.md:35`, `/opt/aspark/templates/plan.md:41-76`, and the session transcript behind E2 (`~/.claude/projects/-workspace-repo/6abcbc86-…/subagents/agent-a89fe96fc6d9876a4.jsonl`). I re-derived E2 from that file under condition (b), because it is the only evidence cited for Must AC-1.1, AC-2.1 and AC-2.3. Lens: `library`. No tool file was passed. Not available here: `claude`, `gh`, `aspark-graph`. The F1 and F18 shapes were probed with a stub shell function. My round-2 fixes (F13 cell, F16-F19) are uncommitted on `factory/issue-83`.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ✅ | All steps present. The F1, F11 and F12 fixes landed in the skill text. |
| T2 | ✅ | Unchanged since round 1. |
| T3 | ⚠️ | The ROADMAP deviation is recorded and the user accepted it (F9). Its stated reason ignores `ROADMAP.md:35` (F20). |
| T4 | ⚠️ | `evidence.md` and its AC table exist. The plan's §4 dry-run claim was ahead of the evidence (F17, fixed). F2 is still open. |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Major | `skills/backlog/SKILL.md:60` | The skill prescribes `--title "<title>" --body "<body>"`, which puts text derived from `docs/` inside double quotes. Followed literally, every Markdown backtick and `$(...)` in a problem statement or title is executed by the shell. I probed this: a body containing `` `id -un` `` and `$(echo INJECTED)` rendered as `factory` and `INJECTED`. This silently mangles every issue that contains inline code, and it violates the constitution §6 rule "nothing is executed … unasked". **Fix:** pass the body with `--body-file -` fed by a single-quoted heredoc (`<<'EOF'`), or from a temp file outside the repo. Single-quote the title and escape `'` inside it. State in the step why. | fixed r2 |
| F2 | Major | `.spark/backlog-seed/plan.md:31` (§4) | Constitution §4 says the change must be "exercised by a documented dogfood or dry run of every phase it touches, negative case first". Nothing was run. The plan's reason ("would write to the real tracker") covers only step 7. The negative case (no `docs/`, no constitution, so it points to `/charter` and writes nothing) and steps 1-6 with the confirmation declined write nothing at all. **Fix:** run the negative case, then a positive dry run that stops at step 6 with a "no". Write both to `.spark/backlog-seed/evidence.md` with an AC table. Optionally file to a scratch repo to cover step 7. **r2:** still only partly done. `evidence.md:9` E1 is "Read only, not run as a session". E2 ran step 2 alone, from a hand-written brief (see F21). Steps 3-6 never ran. A session that could run E2 could also have run E1 in an empty scratch directory. | open |
| F3 | Minor | `skills/backlog/SKILL.md:37` | Step 1 reads `ROADMAP.md`, but step 2 never handed it to the agent, although `agents/product-owner.md:196` expects it. Roadmap items could never be proposed, so AC-1.1 was partial. **Fixed:** added "`ROADMAP.md` if present" to the step 2 brief. | fixed r1 |
| F4 | Minor | `skills/backlog/SKILL.md:40-43` | The drop check only tested whether a reference was *present*, so an invented `docs/x.md#made-up` passed. That defeats AC-2.2. **Fixed:** an item is also dropped when its reference names no file and heading gathered in step 1. The check is still best effort, as disclosed. | fixed r1 |
| F5 | Minor | `skills/backlog/SKILL.md:56-58` (was `:79-80`) | The "say once before filing that bodies are public" line sat in step 8, the post-filing report, so it could only be said after filing (CLAUDE.md, "put a required line where the output is written"). **Fixed:** moved it into the step 6 confirmation. | fixed r1 |
| F6 | Minor | `skills/backlog/SKILL.md:71` | The "exact" body template hardcoded `**Source:** docs/<file>#<section>`, while Rules (`:88-89`) and the agent (`product-owner.md:198`) allow constitution and ROADMAP sources. A constitution-sourced item had no valid body. **Fixed:** the placeholder now names all three source kinds. | fixed r1 |
| F7 | Minor | `docs/status.md:285`, `docs/family.md:16` | NFR-2 asks for updated ceremony counts. `family.md` still said "11 skills" while there are 12 `skills/*` directories. **Fixed:** "12 skills". **r2:** the `status.md` half of this fix was wrong, and I reverted it under F16. The stale "11 skills" in `.spark/constitution.md` §9 needs `/charter`. | fixed r1 |
| F8 | Minor | `docs/status.md` (capability table, cf. `:116-117`) | `README.md:151` points to `docs/status.md` for per-capability proof state. Comparable new skills such as `/campaign` got a row there; `/backlog` has only the checklist note. **Fix:** after F2, add a row stating what was and was not observed live. **r2:** the row is at `:117`. Its overclaim is fixed under F17. | fixed r2 |
| F9 | Minor | `.spark/backlog-seed/plan.md:25-26` | T3 and T4 are marked `done`, but `ROADMAP.md` is unchanged and there is no evidence AC table. These are silent plan deviations. **Fix:** do them, or record the deviation in the plan. **r2:** the deviation is recorded at `plan.md:29` and the user accepted it. The AC table exists. | fixed r2 |
| F10 | Minor | `.claude-plugin/plugin.json:4` | Library lens §2: `main` (marketplace `source: "./"`) already exposes `/backlog` under the unchanged `0.14.3`, so one version number now names two surfaces. Constitution §5 makes a new optional capability a minor bump. **Fix:** release as `0.15.0` at `/go-live` and do not reuse `0.14.3`. | accepted by user r2 (bump at `/go-live`) |
| F11 | Minor | `skills/backlog/SKILL.md:28-29` | "Every Markdown/text document" in `docs/` is unbounded. In this repo that includes large reports and a `.docx`. The cost is agent attention (constitution §4). **Fix:** gather headings plus the lead paragraph of each section, skip non-text files, and name what was skipped in the step 8 report. **r2:** done at `:28-31`. Step 8 did not list the skipped files yet (F19). | fixed r2 |
| F12 | Minor | `skills/backlog/SKILL.md:76-78` | After a mid-run `gh` failure, the skill lists what was created but gives no path to continue. A re-run re-files everything as duplicates. **Fix:** after the user fixes the cause in the same conversation, offer to file only the items not yet created, re-confirming per step 6. **r2:** done at `:87-88`. | fixed r2 |
| F13 | Minor | `.spark/backlog-seed/plan.md:23` | The task table lacks the protected `Story` column (constitution §3 template contract), so `aspark-graph` would raise `TemplateDriftError`. **Fix:** add a `Story` column (appending is allowed). **r2:** the column was added. I corrected T2's cell from `US-1..5` to `US-2` to match its own Covers. The range form and the absent §2 were not checked against `aspark-graph`, which is not installed. | fixed r2 |
| F14 | Minor | `.spark/backlog-seed/spec.md:10,88` | The spec's Status is `approved` while every SPEC GATE box is unchecked and no user approval is recorded ("no agent passes its own gate", constitution §6). **r2:** the user approved the spec through the factory issue, and that approval is recorded here. The SPEC GATE boxes are still unchecked in `spec.md:89`. | accepted by user r2 |
| F15 | Nit | `skills/spark/SKILL.md:31-33` | The no-constitution branch offers `/charter` and `/next-steps` but not `/backlog`, which runs on `docs/` alone (AC-1.2 stops only when both are missing). **Fix:** mention `/backlog` there when `docs/` exists. **r2:** done at `:32-33`. | fixed r2 |
| F16 | Minor | `docs/status.md:286` | My round-1 F7 edit changed the line to "twelve … campaign". `constitution.md:329` rules that this line counts loop ceremonies, which exclude `/campaign`, so the original "eleven" with backlog was right. **Fixed:** reverted to "eleven", without campaign. | fixed r2 |
| F17 | Major | `docs/status.md:117`, `plan.md:33` | The published proof row said "steps 1-6 dry-run once". Plan §4 said the dry run with the negative case and steps 1-6 is in `evidence.md`. `evidence.md:9-10` shows the negative case read only and step 2 run alone. This is a false reassurance in the proof ledger (constitution §4, "Docs in step"). **Fixed:** both now say what actually ran. | fixed r2 |
| F18 | Minor | `skills/backlog/SKILL.md:64` | With the `<<'EOF'` delimiter, a body line that is exactly `EOF` ends the heredoc and the shell runs the rest. Probed: `$(id -un)` after such a line ran as `factory`. The cause is unlikely, but it reopens F1's class. **Fixed:** delimiter `ASPARK_BACKLOG_BODY`, plus a rule to pick another one if a body line equals it. Re-probed: the text stayed literal. | fixed r2 |
| F19 | Minor | `skills/backlog/SKILL.md:89-92` | Step 1 says "name what was skipped in the step 8 report", but step 8's list did not include it, so the line would rarely be spoken (CLAUDE.md, "put a required line where the output is written"). **Fixed:** the skipped files are now in step 8's list. | fixed r2 |
| F20 | Minor | `ROADMAP.md:35`, `plan.md:29` | The T3 deviation says "no item exists to update", but `ROADMAP.md:35` says "ten ceremonies", while `status.md:286` now counts eleven with backlog. NFR-2's count update is incomplete. **Fix:** change it to "eleven ceremonies", or record why backlog is not counted there. | open |
| F21 | Minor | `evidence.md:10,17` | E2 is cited for AC-2.1. In the transcript, the brief told the PO to read the files itself, against `product-owner.md:196` ("You cannot open files yourself"), and capped the list at 6 items. Items 3 and 5 were sized "S–M", not S/M/L. **Fix:** restate E2 as written, with AC-2.1 at 4 of 6 conforming. Re-run it through the skill's steps 1-3 when doing F2. | open |

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1.1 | `SKILL.md:28-30, 34-37` | ✅ met (after F3) |
| AC-1.2 | `SKILL.md:31-33` | ✅ met by text; not run (F2) |
| AC-2.1 | `SKILL.md:38-40`, `product-owner.md:200-202` | ✅ met |
| AC-2.2 | `SKILL.md:40-43`, `product-owner.md:197-199` | ⚠️ best effort, disclosed (F4 fixed) |
| AC-2.3 | `product-owner.md:202` | ✅ met |
| AC-3.1 | `SKILL.md:38, 56-58`, Rules `:86` | ✅ met |
| AC-3.2 | `SKILL.md:44-47` | ✅ met |
| AC-4.1 | `SKILL.md:50-53, 61-68` | ✅ met r2 (F1, F18 fixed; filing not run live) |
| AC-4.2 | `SKILL.md:69-82` | ✅ met r2 |
| AC-4.3 | `SKILL.md:76-78` | ✅ met (resume gap F12) |
| AC-5.1/5.2/5.3 | `SKILL.md:21-22, 52-55` | ✅ met |
| NFR-1 | Rules `SKILL.md:96-97`; step 7 shape safe | ✅ r2 |
| NFR-2 | `README.md:107`, `workflow.md:30`, `spark/SKILL.md:35`, counts | ⚠️ (F7 fixed; F8, F15 open) |
| NFR-3 | `SKILL.md:42-43`, `workflow.md:33-34` | ✅ disclosed; no rate measured (F2) |

## 5. What Was Checked

- [x] Correctness: every round-1 fix was re-read in the current text, and AC-4.x were re-traced
- [ ] Non-functional: the constitution §4 dry-run bar does not hold yet (F2)
- [x] Error handling: a `gh` failure stops, reports and now offers to file only the rest (F12)
- [x] Security: the heredoc and title shape were probed twice; no execution path remains (F1, F18)
- [ ] Tests: no suite exists by design; the dry run is partial (F2, F21); `claude plugin validate` is unavailable here
- [x] Readability: the skill is still short and ordered

## 6. Verdict

"Changes requested. The fix pass closed the dangerous finding. Step 7 now pipes the body through a single-quoted heredoc, and I probed that shape twice. My only addition was a distinctive delimiter (F18), because a body line reading `EOF` would otherwise have run the text after it as shell. Most Minors are closed in the text. F10 and F14 are recorded as accepted by the user. F2 stays open as a Major. Constitution §4 asks for the negative case first, run and written down, and `evidence.md` says plainly that it was only read. Steps 3-6 never ran, although they write nothing. The fix pass also overclaimed that missing run in the published `docs/status.md` row and in the plan (F17). I corrected both to what actually ran, and I reverted my own wrong round-1 count edit (F16). To close the gate: run E1 and a steps 1-6 dry run declined at step 6, then update `evidence.md`, the `status.md:117` row and F21. F20 is a one-line choice for the user." Still open for the user: the constitution's §2 "eleven slash commands" and §9 "11 skills" need a `/charter` amendment, which the user accepted as out of this loop.

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place; never duplicate it as a second gate.*

- [x] No open Blocker findings
- [ ] No open Major findings (F2 open; not waived)
- [x] Every Must AC traces to implementing code; no constitution non-negotiable violated (F1 fixed r2)
- [x] All plan deviations documented and accepted (F9; F20 is about the deviation's stated reason, not an undocumented deviation)
- [ ] Test suite runs green (no suite by design; the §4 dry run is partial, F2; `claude plugin validate` unavailable here)
- [x] Line budget respected: Ist 103 / Soll ~150 (excluding HTML comments)
- [ ] Status set to `passed`
