# Spec: website-lens-public-run

<!-- aspark-template: aspark/0.1.0 -->

| | |
|---|---|
| **Phase** | Specify |
| **Owner** | Product Owner (`/story-time`), drafted by the Factory line `aspark` in dark mode |
| **Status** | `approved` |
| **Date** | 2026-10-28 |
| **Ticket** | #96 |

**Handoff**
- **Status:** `approved` (dark-mode factory line took the recommended gate; the issue's AC-1…AC-6 are the approved scope).
- **Summary:** Run the Review and QA phases with the `accessibility` lens, activated from a declared constitution profile, on a real public `website` at a pinned commit, and verify the lens-derived checks live in a browser.
- **Open:** `none`
- **Binding ruling:** §4 User Stories.
- **On conflict:** the numbered body wins except `Status`.

## 1. Problem & Goal

- **Problem:** ROADMAP "The one gap that matters most": the lens layer's success definition includes a UI lens firing and being QA-verified on a real `website`. The api half got a partial public run (`.spark/api-lens-public-run/`); the website half has none on `main`. That run also passed the lens text by hand.
- **Goal:** One public, re-runnable run: constitution profile → lens resolved from it → Review → live browser QA, every lens-derived check recorded PASS/FAIL with its observable or `not-verified-live` with a reason.
- **Success signal:** A stranger can clone the target at the pinned commit, copy the committed constitution, serve `dist/`, and compare against `evidence.md`.

## 2. Target Users

Maintainers and adopters deciding whether to trust the UI lenses; the aSPARK author reading the roadmap.

## 3. Assumptions & Open Questions

| # | Assumption / Question | Resolution |
|---|---|---|
| A1 | The target (StartBootstrap Agency theme, MIT) has no `.spark/` trail. The run profiles it in a committed `target-constitution.md`, copied untracked into a scratch clone. The profile (`must-be-accessible`) is an assumption written by the Factory line, not by the maintainers. | Accepted; stated in evidence. |
| A2 | There is no real increment (no PR). The reviewed unit is a whole-file snapshot diff of `dist/index.html` and `dist/js/scripts.js` against the empty tree. | Accepted; stated in evidence. |
| A3 | Firing is instruction-driven. One run is one observation, not a rate (AC-3). | Accepted. |
| A4 | An earlier attempt of this same issue exists on the unmerged remote branch `origin/factory/issue-96`. This run was done fresh and none of its verdicts are copied. | Disclosed in evidence. |
| A5 | The container browser may or may not reach CDNs. | Check in the browser's network log; do not assume. |

## 4. User Stories

### US-1 (Must): Name the target so it can be re-run
**Acceptance criteria:**
- [ ] AC-1: Given `evidence.md`, then it names repo URL, pinned commit, license, diff command, serve command, browser and date, and every cited `file:line` resolves with `git show <commit>:<path>`.

### US-2 (Must): Lens activated from the constitution
**Acceptance criteria:**
- [ ] AC-2: Given `target-constitution.md` declaring `website` + `must-be-accessible`, then the lens file path passed to the reviewer and the QA tester is derived from that profile (frontmatter `triggers` and `phases`), not named in either agent's prompt as a choice. Evidence says exactly how dispatch happened and who wrote the profile.

### US-3 (Must): Review fires without a trace request
**Acceptance criteria:**
- [ ] AC-3: Given the reviewer prompt, then it contains no per-item trace request and no lens item names; `evidence.md` records which lens Review items surfaced anyway, as an n=1 figure with no rate claim.

### US-4 (Must): Live QA with binary verdicts
**Acceptance criteria:**
- [ ] AC-4: Given the running page in a real browser, then each lens-derived check (`lenses/accessibility.md` §5) is recorded PASS or FAIL with its observable, or `not-verified-live` with a reason, per `standards/anti-generosity.md`.

### US-5 (Must): Docs claim only what ran
**Acceptance criteria:**
- [ ] AC-5: `evidence.md` lists what is not shown (single run, single model, prompt-enforced only, profile assumed). `ROADMAP.md` and `docs/status.md` match it; nothing is marked Shipped beyond what ran.
- [ ] AC-6: No executable code is added; no lens, skill or agent text changes.

## 5. Non-Functional Requirements

| # | Requirement | How it's verified |
|---|---|---|
| NFR-1 | Accessibility of the target is tested on three named flows: portfolio modal, navbar incl. mobile toggler, contact form; contrast target 4.5:1 body, 3:1 large text. | QA, live |
| NFR-2 | State changes in the contact form (validation error, success) are announced to assistive tech via a live region or equivalent. | QA, live (DOM semantics; no screen reader available) |
| NFR-3 | Negative results are reported as plainly as positive ones. | Review |
| NFR-4 | Scope fence: `.spark/website-lens-public-run/`, `ROADMAP.md`, `docs/status.md`. | Review |

## 6. Out of Scope

Fixing the target; any change to lenses; `seo`/`ux` lenses; a screen-reader run.
