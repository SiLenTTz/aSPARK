# QA Report: backlog-seed

| | |
|---|---|
| **Phase** | Review (hands-on) |
| **Owner** | QA Tester (`/demo-day`) |
| **Input** | Installed-plugin skill `skills/backlog/SKILL.md`, `.spark/backlog-seed/spec.md` (the acceptance criteria) |
| **Status** | `passed` |
| **Round** | 1 |
| **Date** | 2026-10-25 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Not demo-able as a finished flow: steps 1-6 ran live in fresh sessions and behaved, but filing (step 7, AC-4.1/4.2/4.3) could not be run because `gh` is absent. Needs the user to waive that gap (or run it where `gh` exists).
- **Waiver:** user waived AC-4.1/4.2/4.3 and AC-5.2 (not verified live, no `gh`) and accepted B1-B4.
- **Open:** `2 open` - Blockers: none; Majors: none (Minors: B1, B2, see §3). Pending: user waiver of the AC-4.x gap.
- **Binding ruling:** §5 Verdict and the gate checklist below.
- **On conflict:** the numbered body wins for everything except `Status`.

## 1. Test Environment

- **App URL:** N/A. Constitution `.spark/constitution.md` §8: `Browser-observable surface: no`; declared method is hands-on QA against the installed plugin (a performed step = real ceremony invocation or observed command output). Browser, URL and viewport fields are N/A.
- **Tools present:** `command -v claude` and `command -v gh` both returned nothing (rc=1). `git ls-files '*.py'` empty. Sessions were fresh `general-purpose` sub-agents handed `SKILL.md`, not a real `/backlog` slash invocation (no `claude -p`). The `product-owner` agent was invoked for real in the positive session.
- **Test data:** positive case in `/workspace/repo` (origin `https://github.com/a-lottes/aSPARK.git`, no `.spark/backlog.yml`); negative case in `/tmp/scratch-neg` (empty, `git init`, no remote). User answers in §2 marked SIMULATED were typed by me, not a human.
- **Prior evidence** (`evidence.md` E1-E8) was read and not counted.

## 2. Acceptance Criteria Verification

| Spec ID | Steps performed | Expected | Observed | Result |
|---|---|---|---|---|
| AC-1.1 | Fresh session, `/backlog` in repo, steps 1-3,5 | Reads `docs/`, constitution, `ROADMAP.md` | Session reported reading 5 docs files + `docs/reports/README.md`, the constitution and `ROADMAP.md`; skipped `.docx` and 3 `.json` and named them. Self-report of a sub-agent, not independently audited | ✅ pass |
| AC-1.2 | Fresh session in `/tmp/scratch-neg` (empty); I then ran `ls -A` | Names `/charter`, invokes nothing, writes nothing | Named `/charter`, did not invoke it, no `product-owner` call, no `gh`; dir still only `.git` | ✅ pass |
| AC-2.1 | Inspect step-3 table from the positive session | Title, problem, S/M/L, deps, `docs/<file>#<section>` per item | 7 items, all columns present, sizes single letters, sources like `docs/status.md#The open gap`. Two sources are `ROADMAP.md#...` (allowed by SKILL.md, not literally `docs/` per AC). Some source cells list two refs | ✅ pass |
| AC-2.2 | Same table; the session said it checked each source against gathered headings and dropped none | No sourceless item | All 7 carry sources; I did not re-open every heading myself (best effort, prompt-material) | ⚠ partly verified (see B2) |
| AC-2.3 | Inspect Depends column | Deps refer to list items | Items 4-7 refer by number correctly. Items 2/3 read "Soft dependency on 1. Needs a real website." - the second clause is not a list item | ⚠ pass with B1 |
| AC-3.1 | Session stopped before step 6; its confirmation text: "File these 7 issues in a-lottes/aSPARK with no labels? ... Reply yes to file" | Plan shown; nothing created before confirmation | Table shown, question asked, no `gh` run, `git status --short` clean | ✅ pass |
| AC-3.2 | SIMULATED user: "drop item 7, move item 6 to top, reword item 5". Then SIMULATED "abandon" | Edits applied, list re-shown; abandon writes nothing | Edits applied, 6-item table re-shown with renumbered deps and new title; session flagged that order no longer follows dependency. On abandon: stopped, wrote nothing; `git status --short` empty | ✅ pass (simulated user) |
| AC-4.1 | Probed the exact step-7 command with a stub `gh` function and a hostile title/body; ran real `gh` | `gh issue create --repo <owner/repo>`; repo echoed | Command shape safe (backticks and `$(touch ...)` stayed literal, no file created). Repo echo seen at step 5 (`a-lottes/aSPARK`). Real filing impossible: `gh: command not found` | not-verified-live |
| AC-4.2 | Same stub probe printed the body on stdin | Body has problem, size, deps, source, `generation: 0` | Body shape piped through correctly, but a stub is not a created issue and `/story-time <n>` import was not run | not-verified-live |
| AC-4.3 | Ran `gh issue create ...` for real | Stop, report real error, list created issues | Shell returned `bash: gh: command not found`, rc=127. A session did not follow step 7 itself; the text of the stop rule was only read | not-verified-live |
| AC-5.1 | Positive session echoed labels | Default none | "Labels: none" (no args, no `backlog.yml`) | ✅ pass |
| AC-5.2 | Not run with args or a `backlog.yml` | Args win over yml | Neither path run | not-verified-live |
| AC-5.3 | `grep -n -i 'automation\|dependabot\|bot'` on `SKILL.md` | No named automation | Only hit is line 57, "do not assume any particular automation" | ✅ pass |
| NFR-1 | `git status --short` after both sessions; `ls -A /tmp/scratch-neg` | No `.spark/` writes, no issue before confirmation | Clean tree, scratch dir unchanged | ✅ pass (steps 1-6 only; step 7 not run) |
| NFR-2 | `grep` README, workflow, `/spark` skill, status | Ceremony mentioned, counts updated | `README.md:107` row, `docs/workflow.md:30`, `skills/spark/SKILL.md:32,35` mention it. `docs/status.md:286` says "ten ceremony skills ... plus `/backlog`"; no "eleven" anywhere | ✅ pass (see B3) |
| NFR-3 | `grep` for honesty wording | Check documented as instruction | `SKILL.md:44-45` "an instruction to you and the agent, not an enforcement"; `docs/status.md:117` states best effort, no rate | ✅ pass |

## 3. Exploratory Findings

| # | Severity | Steps to reproduce | Expected vs. observed | Status |
|---|---|---|---|---|
| B1 | Minor | Run `/backlog` on this repo; read Depends of items on the website/API runs | Expected: only list items or `none`. Observed: "Soft dependency on 1. Needs a real website." mixes an external prerequisite into the field | accepted by user |
| B2 | Minor | Positive session: the agent's own problem statements were rewritten by the caller session ("I tightened items 1 and 7") | Expected: the agent's list shown as proposed. Observed: caller edited content silently; the source check is not visible to the user | accepted by user |
| B3 | Minor | `grep -rn 'ten ceremon\|eleven' README.md docs/` | Observed: no consistent ceremony count after the skill count rose to 11 (`ls skills` shows 12 dirs including `campaign`); only a "ten plus `/backlog`" phrase. Not rated further (review F20 covers it) | accepted by user |
| B4 | Minor | Run the 3 `docs/reports/*.json` through step 1 | Session itself judged JSON "out of scope"; the skill says skip non-text and name them. It named them, but the call was its own | accepted by user |

## 4. Console & Network

Browser N/A. Command output: `command -v gh` rc=1, `command -v claude` rc=1, `gh issue create` rc=127 `command not found`. No other failing commands. No network request was made.

## 5. Verdict

Verdict, quoted: "Not demo-able as a finished flow: steps 1-6 ran live in fresh sessions and behaved, but filing (step 7, AC-4.1/4.2/4.3) could not be run because `gh` is absent." Would I demo it? Not the filing half. The read-propose-edit-abandon half I would show: the positive case produced a sourced, ordered list, the edit loop re-showed it correctly, abandon wrote nothing, and the negative case stopped at `/charter`. Three Must ACs (AC-4.1-4.3) and the Should AC-5.2 stay `not-verified-live`; I did not pass them from reading. This is a disclosed gap the user must waive (or close by running filing where `gh` is authenticated). Until then Status stays `in-testing`. Sessions were sub-agents following `SKILL.md`, user answers were simulated, and one run each gives no rate.

---

## ✅ QA GATE

- [x] Every Must-story acceptance criterion verified live and passed (AC-4.1, 4.2, 4.3 not-verified-live: **waived by the user, disclosed**; `gh` absent, filing never run live; AC-5.2 likewise)
- [x] Every browser-observable NFR verified and passed (browser N/A; NFR-1..3 checked by command, NFR-1 for steps 1-6 only)
- [x] No open Blocker or Major bugs (B1-B4 are Minor)
- [x] Browser console free of errors on the tested flows (N/A; no errors in command output beyond the expected `gh` missing)
- [x] Tested on all agreed viewports (N/A, not UI-facing)
- [x] Line budget respected: Ist ~78 / Soll ~130 (excluding HTML comments)
- [x] Status set to `passed`
