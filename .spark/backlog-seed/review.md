# Review Report: backlog-seed

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | `git diff 4d6c4ac~1 4d6c4ac` (PR #82, issue #81), `.spark/backlog-seed/plan.md` |
| **Status** | `changes-requested` |
| **Round** | 1 |
| **Date** | 2026-10-25 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Changes requested. Two Majors are open: the `gh` command shape runs the content of `docs/` as shell, and the change was never dry-run as constitution §4 requires.
- **Open:** `10 open`. Blockers: none. Majors: `F1`, `F2` (Minors/Nits: see §3)
- **Binding ruling:** §6 Verdict and the gate checklist below. They are the only binding location; there is no other round to point to.
- **On conflict:** the numbered body below wins for everything except `Status`. Log the mismatch as a finding at the next `/peer-review` and proceed; don't stop on it.

## 1. Scope

Reviewed: `skills/backlog/SKILL.md` (new), `agents/product-owner.md:192-206`, `README.md:107`, `docs/workflow.md:30-34`, `docs/status.md:285`, `skills/next-steps/SKILL.md:92-93`, `skills/spark/SKILL.md:31-36`. For context I also read `skills/story-time/SKILL.md:21-35` (the import shape), `.spark/constitution.md` §2-§6 and §9, and `.claude-plugin/plugin.json`. Lens applied: `library` (§1, §2, §4; §3 is N/A per the constitution). No tool file was passed. Not reviewed: the `.spark/` files of the commit, except to check plan conformance and template shape. Not run: `claude plugin validate`, because the `claude` CLI is not installed in this environment. A live ceremony run was not done either; that is F2. This review is post-merge, since PR #82 is already on `main`. My fixes are uncommitted in the working tree on branch `factory/issue-83`.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ✅ | All the planned steps are present (gather, delegate, edit loop, confirm, file, report), plus rules and handoff. |
| T2 | ✅ | `agents/product-owner.md:192-206` has the item fields, the source rule and the no-invention rule. |
| T3 | ⚠️ | The plan names a ROADMAP update, but `ROADMAP.md` is untouched and no deviation is recorded (F9). The `/next-steps` pointer went into Rules, not into an empty-state branch. That is acceptable but undocumented. |
| T4 | ⚠️ | DoD says "AC table in evidence". No `evidence.md` exists, only a prose self-check (F9, F2). |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Major | `skills/backlog/SKILL.md:60` | The skill prescribes `--title "<title>" --body "<body>"`, which puts text derived from `docs/` inside double quotes. Followed literally, every Markdown backtick and `$(...)` in a problem statement or title is executed by the shell. I probed this: a body containing `` `id -un` `` and `$(echo INJECTED)` rendered as `factory` and `INJECTED`. This silently mangles every issue that contains inline code, and it violates the constitution §6 rule "nothing is executed … unasked". **Fix:** pass the body with `--body-file -` fed by a single-quoted heredoc (`<<'EOF'`), or from a temp file outside the repo. Single-quote the title and escape `'` inside it. State in the step why. | open |
| F2 | Major | `.spark/backlog-seed/plan.md:31` (§4) | Constitution §4 says the change must be "exercised by a documented dogfood or dry run of every phase it touches, negative case first". Nothing was run. The plan's reason ("would write to the real tracker") covers only step 7. The negative case (no `docs/`, no constitution, so it points to `/charter` and writes nothing) and steps 1-6 with the confirmation declined write nothing at all. **Fix:** run the negative case, then a positive dry run that stops at step 6 with a "no". Write both to `.spark/backlog-seed/evidence.md` with an AC table. Optionally file to a scratch repo to cover step 7. | open |
| F3 | Minor | `skills/backlog/SKILL.md:37` | Step 1 reads `ROADMAP.md`, but step 2 never handed it to the agent, although `agents/product-owner.md:196` expects it. Roadmap items could never be proposed, so AC-1.1 was partial. **Fixed:** added "`ROADMAP.md` if present" to the step 2 brief. | fixed r1 |
| F4 | Minor | `skills/backlog/SKILL.md:40-43` | The drop check only tested whether a reference was *present*, so an invented `docs/x.md#made-up` passed. That defeats AC-2.2. **Fixed:** an item is also dropped when its reference names no file and heading gathered in step 1. The check is still best effort, as disclosed. | fixed r1 |
| F5 | Minor | `skills/backlog/SKILL.md:56-58` (was `:79-80`) | The "say once before filing that bodies are public" line sat in step 8, the post-filing report, so it could only be said after filing (CLAUDE.md, "put a required line where the output is written"). **Fixed:** moved it into the step 6 confirmation. | fixed r1 |
| F6 | Minor | `skills/backlog/SKILL.md:71` | The "exact" body template hardcoded `**Source:** docs/<file>#<section>`, while Rules (`:88-89`) and the agent (`product-owner.md:198`) allow constitution and ROADMAP sources. A constitution-sourced item had no valid body. **Fixed:** the placeholder now names all three source kinds. | fixed r1 |
| F7 | Minor | `docs/status.md:285`, `docs/family.md:16` | NFR-2 asks for updated ceremony counts. `status.md` said "eleven" and listed 11 skills while omitting `campaign`; there are 12 `skills/*` directories. `family.md` still said "11 skills". **Fixed:** "twelve … campaign" and "12 skills". The stale "11 skills" in `.spark/constitution.md` §9 can be amended only by `/charter` (see §6). | fixed r1 |
| F8 | Minor | `docs/status.md` (capability table, cf. `:116-117`) | `README.md:151` points to `docs/status.md` for per-capability proof state. Comparable new skills such as `/campaign` got a row there; `/backlog` has only the checklist note. **Fix:** after F2, add a row stating what was and was not observed live. | open |
| F9 | Minor | `.spark/backlog-seed/plan.md:25-26` | T3 and T4 are marked `done`, but `ROADMAP.md` is unchanged and there is no evidence AC table. These are silent plan deviations. **Fix:** do them, or record the deviation in the plan. | open |
| F10 | Minor | `.claude-plugin/plugin.json:4` | Library lens §2: `main` (marketplace `source: "./"`) already exposes `/backlog` under the unchanged `0.14.3`, so one version number now names two surfaces. Constitution §5 makes a new optional capability a minor bump. **Fix:** release as `0.15.0` at `/go-live` and do not reuse `0.14.3`. | open |
| F11 | Minor | `skills/backlog/SKILL.md:28-29` | "Every Markdown/text document" in `docs/` is unbounded. In this repo that includes large reports and a `.docx`. The cost is agent attention (constitution §4). **Fix:** gather headings plus the lead paragraph of each section, skip non-text files, and name what was skipped in the step 8 report. | open |
| F12 | Minor | `skills/backlog/SKILL.md:76-78` | After a mid-run `gh` failure, the skill lists what was created but gives no path to continue. A re-run re-files everything as duplicates. **Fix:** after the user fixes the cause in the same conversation, offer to file only the items not yet created, re-confirming per step 6. | open |
| F13 | Minor | `.spark/backlog-seed/plan.md:23` | The task table lacks the protected `Story` column (constitution §3 template contract), so `aspark-graph` would raise `TemplateDriftError`. §2 of the template is also missing. **Fix:** add a `Story` column (appending is allowed). | open |
| F14 | Minor | `.spark/backlog-seed/spec.md:10,88` | The spec's Status is `approved` while every SPEC GATE box is unchecked and no user approval is recorded ("no agent passes its own gate", constitution §6). **Fix:** record the user's approval, or check the boxes that hold. See the open question in §6. | open |
| F15 | Nit | `skills/spark/SKILL.md:31-33` | The no-constitution branch offers `/charter` and `/next-steps` but not `/backlog`, which runs on `docs/` alone (AC-1.2 stops only when both are missing). **Fix:** mention `/backlog` there when `docs/` exists. | open |

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
| AC-4.1 | `SKILL.md:49-51, 59-60` | ⚠️ partial: the command shape is unsafe (F1) |
| AC-4.2 | `SKILL.md:61-74` | ⚠️ partial: the body is mangled by quoting (F1) |
| AC-4.3 | `SKILL.md:76-78` | ✅ met (resume gap F12) |
| AC-5.1/5.2/5.3 | `SKILL.md:21-22, 52-55` | ✅ met |
| NFR-1 | Rules `SKILL.md:86-87`; F1 is an execution path | ⚠️ |
| NFR-2 | `README.md:107`, `workflow.md:30`, `spark/SKILL.md:35`, counts | ⚠️ (F7 fixed; F8, F15 open) |
| NFR-3 | `SKILL.md:42-43`, `workflow.md:33-34` | ✅ disclosed; no rate measured (F2) |

## 5. What Was Checked

- [x] Correctness: every AC was traced to its text, and the import shape was checked against `story-time/SKILL.md:33`
- [ ] Non-functional: NFR-1 (F1) and the constitution §4 dogfood bar (F2) do not hold
- [x] Error handling: a `gh` or `origin` failure stops and reports; nothing is swallowed
- [ ] Security: text derived from `docs/` reaches a shell unquoted (F1)
- [ ] Tests: no suite exists by design; the required dry run is missing (F2); `plugin validate` could not run here
- [x] Readability: the skill is short, ordered and mirrors `/next-steps`

## 6. Verdict

"Changes requested. The `/backlog` skill is a sound, small and well-disclosed addition, and it traces to every Must AC. I applied five low-risk fixes (F3-F7). Two Majors keep the gate closed. F1: the prescribed `--body "<body>"` shape executes any backtick or `$(...)` from the project's documents. I confirmed this with a probe. It corrupts every issue body that contains inline code and contradicts the non-negotiable against executing anything unasked. F2: the change merged without the dry run that constitution §4 requires, although the negative case and steps 1-6 write nothing and could have been run safely. The remaining Minors are documentation and plan drift. They should be closed in the same fix pass, but none of them blocks on its own." Open questions for the user: (a) was the spec's `approved` status (F14) given by you? (b) PR #82 merged before any review or QA; should the release wait on this round? (c) the constitution's §9 "11 skills" is now stale and needs a `/charter` amendment.

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place; never duplicate it as a second gate.*

- [x] No open Blocker findings
- [ ] No open Major findings (F1, F2 open; not waived)
- [ ] Every Must AC traces to implementing code; no constitution non-negotiable violated (AC-4.1/4.2 partial via F1)
- [ ] All plan deviations documented and accepted (F9)
- [ ] Test suite runs green (no suite by design; the §4 dry run is missing, F2; `claude plugin validate` unavailable here)
- [x] Line budget respected: Ist 97 / Soll ~150 (excluding HTML comments)
- [ ] Status set to `passed`
