# Review Report: website-lens-public-run

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | Target snapshot `b2d5d5c` (`dist/index.html`, `dist/js/scripts.js`) vs empty tree; `.spark/website-lens-public-run/plan.md` |
| **Status** | `passed` |
| **Round** | 1 |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** The run's own artifacts pass for T2 with 5 open Minors (disclosure items for `evidence.md`). The target has 3 open Majors and 4 lower findings, which go to the target and are not routed to `/increment` (spec §6).
- **Open:** run artifacts: Blockers: none; Majors: none. Target: Majors: `F8`, `F9`, `F10` (Minors/Nits: see §3)
- **Binding ruling:** §6 Verdict and the gate checklist below are the only binding location. There is no other round to point to.
- **On conflict:** the numbered body below wins for everything except `Status`. Log the mismatch as a finding at the next `/peer-review` and proceed. Do not stop on it.

## 1. Scope

- **Reviewed unit:** there is **no real PR or increment** (spec A2). I reviewed the whole files `dist/index.html` (610 lines) and `dist/js/scripts.js` (54 lines) of `https://github.com/StartBootstrap/startbootstrap-agency` at `b2d5d5c6b1396bc5b0e8dd92c693b106cfb7cee1` (MIT, `LICENSE` l.1). The diff was `git diff 4b825dc642cb6eb9a060e54bf8d69288fbee4904 b2d5d5c -- dist/index.html dist/js/scripts.js` (664 insertions). Every `dist/...:<n>` below was resolved with `git show b2d5d5c:<path> | sed -n '<n>p'`.
- **Not reviewed:** `src/` (out of scope). Also not reviewed: the CDN code the page loads (`bootstrap.bundle.min.js` 5.2.3 at `dist/index.html:601`, `sb-forms-latest.js` at `:608`, Font Awesome at `:12`). It is not in the unit, and modal focus behaviour depends on it.
- **Mode: static read only.** I did not serve, open or run the page in this phase. Every keyboard and focus item therefore reads `⚠ not-verified-live`, and its result is degraded relative to what the item needs (anti-generosity rule 3).
- **Governing constitution:** `/tmp/target/startbootstrap-agency/.spark/constitution.md`. It is byte-identical to the committed `.spark/website-lens-public-run/target-constitution.md` (`diff` gave no output). Active lenses per its §2 are `accessibility` only. `seo` and `ux` are switched off there on purpose.
- **Lens resolution:** I read the frontmatter of all 9 files in `/opt/aspark/lenses/`. Only `accessibility.md` is active, and it declares `phases: [specify, design, act, review, qa]`, so I applied its §4 (Review) rows. The constitution contains **no section named a pass-through rule** (it has §§1–4 and §8 only). I applied the skill's own rule instead (`/opt/aspark/skills/peer-review/SKILL.md:45-50`: pass every active lens whose `phases` include `review`). See F4.
- **Standards applied:** **Anti-generosity standard applied** (`/workspace/repo/standards/anti-generosity.md`, read in full before the verdict). It is not shipped in `/opt/aspark` (see F3).
- **Tool file:** none passed. I did not use any graph tool.
- **Target untouched:** `git status` in the target shows only `?? .spark/`. In this repo, `git status` shows only `?? .spark/website-lens-public-run/`.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | PASS (constitution copy) / `⚠ not-verified-live` (serve) | The constitution copy is identical (`diff`). I did not check the HTTP 200 on `:8766` because this phase is static only. |
| T2 | PASS | This report. The lens was resolved from the constitution, and the prompt names no lens items (see AC-2 and AC-3). The plan row still reads `todo` at `plan.md:42`. See F1. |
| T3–T5 | not started | Status `todo` (`plan.md:43-45`). These tasks are not due in this phase. |

## 3. Findings

The Location column marks which unit each finding belongs to: **[run]** is this repo's `.spark/` artifacts and **[target]** is the pinned snapshot. For target rows, "lens §4" means the finding surfaced from the accessibility lens Review slice. "Outside §4" means I observed it, but the lens assigns it to Act or QA (`lenses/accessibility.md` §4 bullet 3).

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Minor | [run] `plan.md:42`, `SKILL.md:22` | **Problem:** the skill's step-1 gate requires all plan tasks to be `done`. This plan lists `/peer-review` itself as T2, followed by T3–T5, so the gate cannot hold by construction, and the review ran with only T1 `done`. **Why it matters:** an unstated gate bypass. **Fix:** `evidence.md` states that the gate was applied to T1 only, by design. | open |
| F2 | Minor | [run] `SKILL.md:45-50` vs this run | **Problem:** the skill says the *skill* reads the lenses and passes their paths to the reviewer. In this run the caller told the reviewer to resolve them from the constitution itself, and no lens name was given. **Why it matters:** AC-2 holds, but AC-2 also requires the evidence to say "exactly how dispatch happened". **Fix:** record it in `evidence.md` as "agent-side resolution by instruction, not skill-side path passing". | open |
| F3 | Minor | [run] `/opt/aspark/.claude-plugin/plugin.json` (0.14.2) vs repo (0.16.0) | **Problem:** the run uses the installed plugin at v0.14.2. Its `peer-review/SKILL.md` lacks the anti-generosity pass that the repo has at `skills/peer-review/SKILL.md:56-58`, so the caller passed the standard by hand. The lens file itself is identical (`diff -q`). **Why it matters:** reproducibility (AC-1) and an honest "not shown" list (AC-5). **Fix:** `evidence.md` names the plugin version and the hand-passed standard. | open |
| F4 | Minor | [run] `target-constitution.md` (headings at l.10, 14, 28, 32, 36) | **Problem:** the constitution has no pass-through rule and no §§5–7. **Why it matters:** the caller referred to a rule that does not exist, and a reader re-running the work could not find it. **Fix:** `evidence.md` names the rule actually used (`SKILL.md:45-50`, `phases` includes `review`) and notes that the constitution is a deliberately partial profile. | open |
| F5 | Minor | [run] `git branch -vv`: `factory/issue-96` tracks `origin/main` | **Problem:** the run's work is uncommitted. Meanwhile `origin/factory/issue-96` (`2ea46f7`) holds the earlier attempt (spec A4). **Why it matters:** CLAUDE.md:35 says to commit to a real branch at the start of a loop, and a push to the same-named remote branch will be non-fast-forward. The branch is not stale today (HEAD = `origin/main` = `ec48198`). **Fix:** commit now, and decide the remote branch name before `/go-live`. | open |
| F6 | Minor | [target] `dist/js/scripts.js:46-50` (lens §4) | **Problem:** at mobile width, a click on a nav link calls `navbarToggler.click()`, which collapses `#navbarResponsive` and hides the link that has focus. Nothing moves focus afterwards, so it may drop to `<body>`. **Why it matters:** keyboard users lose their place after in-page navigation. **Fix:** focus the target section (`tabindex="-1"`) or the toggler. **For QA:** at narrow width, press Enter on "Contact" and read `document.activeElement`. `⚠ not-verified-live` | open |
| F7 | Minor | [target] `dist/index.html:394` (same at `:429,:464,:499,:534,:569`) (lens §4) | **Problem:** the visible "X" close control is a `<div data-bs-dismiss="modal">` with no `tabindex` and no `role`, so it cannot be reached by keyboard. Esc is not disabled (no `data-bs-keyboard`/`data-bs-backdrop` anywhere: `grep` count 0), and "Close Project" (`:414`) is a `<button>`. **Why it matters:** this is not a trap by static read, but one of the dialog's exits is mouse-only. **Fix:** make it a `<button type="button" class="close-modal">`. `⚠ not-verified-live` | open |
| F8 | Major | [target] `dist/index.html:327,332,338,345` (outside §4) | **Problem:** none of the four contact fields has a `<label>`, `aria-label` or `aria-labelledby` (`grep '<label'` count 0). The placeholder is the only name. **Why it matters:** WCAG 1.3.1 / 3.3.2 / 4.1.2 at the constitution's AA floor (§4). The name vanishes on input. **Fix:** add a visible or `visually-hidden` `<label for>`. | open |
| F9 | Major | [target] `dist/index.html:354,366`, `:328,333,334,339,346` (outside §4; NFR-2) | **Problem:** the success and error containers have no `aria-live`, `role="status"` or `role="alert"`. The `invalid-feedback` messages are not tied to their fields by `aria-describedby` (`grep` count 0). `sb-forms-latest.js` (`:608`) might add these at runtime, and that cannot be seen statically. **Why it matters:** if the runtime does not add them, NFR-2 fails, because state changes are sighted-only. **Fix:** `role="status"` on `#submitSuccessMessage`, `role="alert"` on `#submitErrorMessage`, `aria-describedby` on the inputs. `⚠ not-verified-live`: QA decides it. | open |
| F10 | Major | [target] `dist/index.html:608`, `:601`, `:12` (baseline security) | **Problem:** a third-party script is loaded unpinned (`sb-forms-latest.js`) with no `integrity`, and it reads the name, email and phone fields. The Bootstrap and Font Awesome CDN scripts also lack SRI. **Why it matters:** this is a supply-chain risk. Whoever controls that URL can change the script and read what users type, and the page cannot detect it. **Fix:** pin a version and add `integrity` plus `crossorigin`. | open |
| F11 | Minor | [target] `dist/index.html:23`, `:391` (outside §4) | **Problem:** the only content of the brand link is `<img alt="...">`, so its accessible name is "...". The six modals have `role="dialog"` but no `aria-labelledby` pointing to their `<h2>` (`:400` etc.). **Why it matters:** screen-reader users hear meaningless names. **Fix:** give the logo a real `alt`, and add `aria-labelledby` to each modal. | open |
| F12 | Nit | [target] `dist/js/scripts.js:42,48` | **Problem:** `navbarToggler` is never null-checked, unlike `#mainNav` at `:15` and `:34`. On markup without `.navbar-toggler`, a click on a nav link throws a `TypeError` from `getComputedStyle(null)`. It cannot happen on this page (`:24`). **Fix:** `if (!navbarToggler) return;`. | open |

I fixed nothing myself. The target is read-only for this run (spec §6, NFR-4).

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1 | `evidence.md` (T4, not yet written) | `⚠ not-verified-live`: the artifact does not exist yet. This report's own citations resolve at `b2d5d5c`, which I checked per row. |
| AC-2 | `target-constitution.md` §2 → `accessibility.md` frontmatter `triggers`/`phases`; reviewer prompt | PASS for the Review half. The prompt names no lens. I derived the lens from `must-be-accessible`, and the profile's author is stated at `target-constitution.md` (Owner row). The QA half is due at T3. |
| AC-3 | reviewer prompt | PASS. The prompt contains no per-item trace request and no lens item names. Items that surfaced anyway: §4 bullet 1 (keyboard trap → F7), §4 bullet 2 (focus on navigation/modal → F6 and the lens row below). n = 1, so this is not a rate. |
| AC-4 | `qa.md` (T3) | `⚠ not-verified-live`: QA has not run. |
| AC-5, AC-6 | `evidence.md`, `ROADMAP.md`, `docs/status.md` (T4, T5) | `⚠ not-verified-live`: not written yet. AC-6 holds so far (`git status` shows `.spark/` only). |
| NFR-1 | flows exist: modal `:391`, toggler `:24`, form `:322` | `⚠ not-verified-live` (QA, live) |
| NFR-2 | no live region in markup (F9) | `⚠ not-verified-live`. The static observable points to FAIL, and QA decides. |
| NFR-3 | this report | PASS. Negative results are stated as findings F6–F12. |
| NFR-4 | `git status` (both repos) | PASS so far: only `.spark/website-lens-public-run/` is changed. |
| Lens §4.1 no keyboard trap | modals `:391…:566`, Bootstrap focustrap (CDN, not in unit) | `⚠ not-verified-live`. The diff adds no custom trap, and Esc is not disabled. Trap release is library code I did not read. |
| Lens §4.2 focus on modal close / navigation | modal trigger `:93`, `scripts.js:46-50` | `⚠ not-verified-live`. Focus return depends on Bootstrap's data-api (CDN). The navbar risk is F6. |

## 5. What Was Checked

- [x] Correctness: I read both scripts.js handlers line by line. F12 is the only logic defect.
- [x] Non-functional: I checked the constitution's AA floor and NFR-1/2/4 statically. The live parts are deferred to QA and marked as such.
- [x] Error handling: there are no `catch` blocks in the unit. The error path of the form is markup only (`:366`), see F9.
- [x] Security: no secrets in the unit (`API_TOKEN` at `:322` is a placeholder). Supply chain: F10.
- [x] Tests: the target has no test suite in this unit. The plan's §4 strategy (re-resolve citations, `git status` fence) was run and holds.
- [x] Readability: the unit is small template code, with one Nit.

## 6. Verdict

**Run artifacts: pass for T2.** The lens was activated from the committed, assumed profile and not from my prompt. The `accessibility` lens's two Review items both surfaced without being requested (F6, F7, §4 lens rows). The scope fence holds, and every `file:line` here resolves at `b2d5d5c`. The run still has five open Minors (F1–F5). Each one is something `evidence.md` must disclose: the bypassed all-tasks-done gate, agent-side rather than skill-side lens resolution, the installed plugin being 0.14.2 with the anti-generosity standard passed by hand, a "pass-through rule" that does not exist in the constitution, and uncommitted work beside a same-named remote branch. None of them changes a gate answer. AC-1, AC-4 and AC-5 are not certified here, because their artifacts do not exist yet. **Target: changes requested in the target, not in this run.** It has three open Majors: unlabelled form fields (F8), no live region for form state (F9, NFR-2, likely FAIL pending QA) and an unpinned third-party form script without SRI (F10). It also has three Minors (F6, F7, F11) and one Nit (F12). This review was a static read only, so every keyboard and focus verdict is `⚠ not-verified-live` and is owed to `/demo-day`. Per the caller's instruction and spec §6, target defects do not gate this run. I have not waived them. They stay open and are routed to `evidence.md` as target findings.

---

## REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`.*

- [x] No open Blocker findings (run or target)
- [x] No open Major findings **on run artifacts**. The target Majors F8–F10 are open and not waived. They are excluded from this gate by the caller's instruction and spec §6 (fixing the target is out of scope), not by a waiver.
- [x] Every Must AC in T2's `Covers` (AC-2, AC-3) traces and PASSes. No constitution non-negotiable is violated by the run. AC-1, AC-4 and AC-5 are not certified (T3–T5 not run).
- [x] All plan deviations documented (F1, F2). Acceptance is pending disclosure in `evidence.md`.
- [x] Test suite runs green: the target has no executable suite, so the plan §4 test strategy is this run's suite. Its checks (citation re-resolution, fence) were run and hold.
- [x] Line budget respected: Ist 100 / Soll ~150 (excluding HTML comments)
- [x] Status set to `passed`. This is for the run's T2 and is not a statement about the target's accessibility.
