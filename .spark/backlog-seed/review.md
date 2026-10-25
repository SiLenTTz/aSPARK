# Review Report: backlog-seed

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | `git diff 4d6c4ac~1 4d6c4ac` (PR #82, issue #81), fix commits `120dfb1`, `4c61403`, `18988c4`, `db917dd`, `cf840ac`, `e8d8cd2`, `749e42f`, `.spark/backlog-seed/plan.md`, `evidence.md` |
| **Status** | `changes-requested` |
| **Round** | 4 |
| **Date** | 2026-10-25 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Changes requested for one gate item only. F2 is fixed r4 with disclosed limits (abridged list, a brief that pointed at the negative stop). No Major is open. The test box stays open because `claude plugin validate .` has never run for this feature (there is no CLI here). F20 is reopened (a count conflict, Minor).
- **Open:** `1 open`. Blockers: none. Majors: none (Minors/Nits: see §3)
- **Binding ruling:** §6 Verdict and the gate checklist below. They are the only binding location; there is no other round to point to.
- **On conflict:** the numbered body below wins for everything except `Status`. Log the mismatch as a finding at the next `/peer-review` and proceed; don't stop on it.

## 1. Scope

Round 4 re-reviewed `cf840ac`, `e8d8cd2` and `749e42f`: `evidence.md` E7/E8 and the AC table, `ROADMAP.md:35`, `docs/status.md:117,182,286` and `plan.md:33`. F2, F20 and F24 were re-derived under condition (a), verifying a fix. E7/E8 were also re-derived under condition (b), because they are the only evidence for Must AC-1.2, AC-3.1 and AC-3.2. Sources: the main session `~/.claude/projects/-workspace-repo/6abcbc86-….jsonl` (events 242-261; the E8 questions are at 248/253 and 260/261) and the subagent `agent-a8dcc05e14fc1b46e.jsonl` (E7, read in full). Also checked: `constitution.md:154,303,329`; both manifests parse as JSON and this feature does not touch them; the `SKILL.md` and `product-owner.md` frontmatter parses as YAML. Lens: `library`. No tool file was passed. Not available here: `claude`, `gh`, `aspark-graph`. My round-4 edits (F25-F27, the E8 disclosure) are uncommitted.

**Open question:** both E8 answers (and E6's) were the option the session itself had labelled "(Recommended)". The transcript cannot show whether a human or an auto-responder chose them, so "real user" rests on the caller's word.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ✅ | All steps present. The F1, F11 and F12 fixes landed in the skill text. |
| T2 | ✅ | Unchanged since round 1. |
| T3 | ⚠️ | `ROADMAP.md:35` was reverted to ten in r4. `docs/status.md:286` still counts eleven with backlog (F20 open). |
| T4 | ✅ | E7/E8 complete the dry run, with its limits disclosed (F2 fixed r4). §4 now names the abridged list (F26). |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Major | `skills/backlog/SKILL.md:60` | The skill prescribes `--title "<title>" --body "<body>"`, which puts text derived from `docs/` inside double quotes. Followed literally, every Markdown backtick and `$(...)` in a problem statement or title is executed by the shell. I probed this: a body containing `` `id -un` `` and `$(echo INJECTED)` rendered as `factory` and `INJECTED`. This silently mangles every issue that contains inline code, and it violates the constitution §6 rule "nothing is executed … unasked". **Fix:** pass the body with `--body-file -` fed by a single-quoted heredoc (`<<'EOF'`), or from a temp file outside the repo. Single-quote the title and escape `'` inside it. State in the step why. | fixed r2 |
| F2 | Major | `.spark/backlog-seed/plan.md:31` (§4) | Constitution §4 says the change must be "exercised by a documented dogfood or dry run of every phase it touches, negative case first". Nothing was run. The plan's reason ("would write to the real tracker") covers only step 7. The negative case (no `docs/`, no constitution, so it points to `/charter` and writes nothing) and steps 1-6 with the confirmation declined write nothing at all. **Fix:** run the negative case, then a positive dry run that stops at step 6 with a "no". Write both to `.spark/backlog-seed/evidence.md` with an AC table. Optionally file to a scratch repo to cover step 7. **r2:** still only partly done. `evidence.md:9` E1 is "Read only, not run as a session". E2 ran step 2 alone, from a hand-written brief (see F21). Steps 3-6 never ran. **r3:** steps 1, 2, 5 and 6 ran (E6, transcript events 165-186), and a real user answered no at step 6. But the PO's list went straight into a yes/no question: it was never shown as a plan (step 3), and no edit was offered (step 4, Must US-3). The user declined a list they never saw. E5 ran only `ls`, so the `/charter` stop is still read only. **r4:** E7 ran the stop as a session (transcript: `ls`, named `/charter`, wrote nothing), but its brief pointed at the stop (F25). E8 (events 248-261) showed the list, applied "drop item 6", re-showed it, and got a no at step 6. Disclosed limits: the list was abridged (no problem statements, and the re-shown list had no sources), and the negative case ran after the positive one (F27). | fixed r4 |
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
| F20 | Minor | `ROADMAP.md:35`, `plan.md:29` | The T3 deviation says "no item exists to update", but `ROADMAP.md:35` says "ten ceremonies", while `status.md:286` now counts eleven with backlog. NFR-2's count update is incomplete. **Fix:** change it to "eleven ceremonies", or record why backlog is not counted there. **r4:** `cf840ac` reverted `ROADMAP.md:35` to ten because "backlog is not a loop ceremony" (the caller's reason; no user ruling is recorded). But `status.md:286` still counts eleven with backlog, and `constitution.md:329` says both lines count the same set. That set already includes `/next-steps` and `/charter`, which also sit outside the five phases, so the stated reason does not set backlog apart. **Fix (the user decides):** eleven at `ROADMAP.md:35`, or ten at `:286` with backlog listed separately. The T3 cell at `plan.md:29` still says "ten to eleven" and must follow the decision. | open |
| F21 | Minor | `evidence.md:10,17` | E2 is cited for AC-2.1. In the transcript, the brief told the PO to read the files itself, against `product-owner.md:196` ("You cannot open files yourself"), and capped the list at 6 items. Items 3 and 5 were sized "S–M", not S/M/L. **Fix:** restate E2 as written, with AC-2.1 at 4 of 6 conforming. Re-run it through the skill's steps 1-3 when doing F2. **r3:** E2 is no longer counted for AC-2.1, and E6's output conforms 6 of 6 (one-letter sizes, dependencies within the list). Its brief has the same defect (F23). | fixed r3 |
| F22 | Major | `docs/status.md:117`, `evidence.md:13`, `plan.md:33` | The fix pass published "steps 1-6 run once in order" in the proof ledger and "ran the negative case and steps 1-6" in the plan. The transcript shows no list shown (step 3), no step 4, and E5 was only `ls`. This is the same false reassurance as F17 (constitution §4, "Docs in step"). **Fixed:** all three now name the steps that actually ran. | fixed r3 |
| F23 | Minor | `evidence.md:13,22` | E6's brief said "Read the files only as needed" and capped the list at 6. It gave headings without lead paragraphs (step 1) and the constitution as a path rather than an excerpt (step 2). The agent read `ROADMAP.md` and grepped `status.md`, against `product-owner.md:196`. Yet the row said "no 'read the files' instruction", and the AC table rejected E2 for this exact defect. **Fixed:** the row and AC table now state the brief as written. Use a brief that follows step 2 when F2 is re-run. | fixed r3 |
| F24 | Minor | `docs/status.md:182` | The loop's Scope row says "10 ceremony skills", and `constitution.md:329` counts it with `ROADMAP.md:35`, which now says eleven. NFR-2's count is still inconsistent. Writing 11 there would put a skill never run live under State "Proven". **r4:** `:182` and `ROADMAP.md:35` now both say ten. The remaining conflict with `:286` is tracked under F20. | fixed r4 |
| F25 | Minor | `evidence.md:14` | E7 said the agent "was told only '/backlog'". The brief (event 242) also said "follow SKILL.md step 1 and its stop rule literally", which points the agent at the very stop being tested, so the evidence is weaker than the row said. **Fixed:** the row now quotes the brief. | fixed r4 |
| F26 | Minor | `docs/status.md:117,286`, `plan.md:33` | The proof row disclosed that step 3 was abridged, but not that step 1 gathered headings only or that step 2's brief let the PO read files (F23). `:286` still said "never run live". Plan §4 did not mention the abridged list. **Fixed:** all three now say so. | fixed r4 |
| F27 | Nit | `evidence.md:14` | Constitution §4 (`:154`) asks for the "negative case first". E7 ran after E6, and E8 started before E7 had finished. This cannot be fixed after the fact. **Fixed:** the order is disclosed in E7. | fixed r4 |

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1.1 | `SKILL.md:28-30, 34-37` | ✅ met (after F3) |
| AC-1.2 | `SKILL.md:31-33` | ✅ met r4; one session (E7), brief pointed at the stop (F25) |
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
| NFR-2 | `README.md:107`, `workflow.md:30`, `spark/SKILL.md:35`, counts | ⚠️ r3 (F20 open again r4; F24 fixed r4) |
| NFR-3 | `SKILL.md:42-43`, `workflow.md:33-34` | ✅ disclosed; no rate measured (F2) |

## 5. What Was Checked

- [x] Correctness: F2, F20 and F24 were re-derived, and E7/E8 were re-derived from the transcripts
- [x] Non-functional: the constitution §4 dry run now covers steps 1-6 and the negative stop, with its limits disclosed (F2, F25-F27); NFR-2 count conflict open (F20)
- [x] Error handling: a `gh` failure stops, reports and now offers to file only the rest (F12)
- [x] Security: the heredoc and title shape were probed twice; no execution path remains (F1, F18)
- [ ] Tests: no suite exists by design; the dry run holds; `claude plugin validate .` (constitution `:303`) never ran for this feature. Manifests are untouched and frontmatter parses, but that is a substitute, not the bar
- [x] Readability: the skill is still short and ordered

## 6. Verdict

Changes requested, narrowly. The dry run is now honest and complete enough to close F2. A real session in an empty directory stopped at `/charter` and wrote nothing. The list was shown, one edit was applied and re-shown, and filing was declined at step 6. The evidence states its limits: the list was abridged, without problem statements and, on re-showing, without sources, rather than shown as step 3's full table; the negative brief pointed at the stop; the negative case did not run first. I corrected the remaining under-disclosures myself (F25-F27). No Blocker or Major is open. One gate box stays open: the constitution's test bar is `claude plugin validate .` plus the dry run, and validate has never run for this feature because the CLI is not installed here. One green run where the CLI exists, or a user waiver of that box, makes this a pass with no code change. F20 is open again as a Minor: reverting `ROADMAP.md:35` to ten conflicts with `status.md:286`, which counts eleven, and the user should pick one count. It does not block. I record it as open, not as accepted, because only the user accepts findings. Accepted by the user and out of scope: F10, F14, and the constitution §2/§9 counts (`/charter`). The open question in §1 (whether a human chose the "(Recommended)" answers) should be confirmed by the user.

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place; never duplicate it as a second gate.*

- [x] No open Blocker findings
- [x] No open Major findings (F2 fixed r4, limits disclosed; F22 fixed r3)
- [x] Every Must AC traces to implementing code; no constitution non-negotiable violated (F1 fixed r2)
- [x] All plan deviations documented and accepted (F9; the T3 count wording depends on F20, a Minor)
- [ ] Test suite runs green (no suite by design; §4 dry run holds r4; `claude plugin validate .` never run, CLI unavailable here: needs one run or a user waiver)
- [x] Line budget respected: Ist 111 / Soll ~150 (excluding HTML comments)
- [ ] Status set to `passed`
