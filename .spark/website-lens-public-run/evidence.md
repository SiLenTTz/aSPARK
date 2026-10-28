# Evidence: website-lens-public-run

| | |
|---|---|
| Feature | website-lens-public-run |
| Phase | Review / QA (evidence for `review.md` and `qa.md`) |
| Date | 2026-10-28 |
| Ticket | #96 |

## 1. Target and re-run (AC-1)

| | |
|---|---|
| Repo | `https://github.com/StartBootstrap/startbootstrap-agency` (MIT), Agency theme |
| Pinned commit | `b2d5d5c6b1396bc5b0e8dd92c693b106cfb7cee1` (2023-03-24) |
| Reviewed unit | `dist/index.html` (610 lines) and `dist/js/scripts.js` (54 lines) as a whole-file diff against the empty tree: `git diff 4b825dc642cb6eb9a060e54bf8d69288fbee4904 b2d5d5c -- dist/index.html dist/js/scripts.js`. No real PR exists; `src/` not reviewed |
| Re-run: clone | `git clone https://github.com/StartBootstrap/startbootstrap-agency && cd startbootstrap-agency && git checkout b2d5d5c` |
| Re-run: constitution | copy `.spark/website-lens-public-run/target-constitution.md` (this repo) to `.spark/constitution.md` in the clone, untracked |
| Re-run: serve | `cd dist && python3 -m http.server 8766`, open `http://127.0.0.1:8766/` (the unmodified page; this run did not alter it) |
| Browser | Playwright MCP headless Chromium; 1280x800 and 390x844 |
| Run | 2026-10-28; `claude-sonnet-5-5` for orchestrator, `reviewer` and `qa-tester` |
| Citations | `git show b2d5d5c:dist/index.html \| sed -n '<n>p'`. Re-resolved by me: `:23` (`alt="..."`), `:327` (placeholder-only field), `:354`/`:366` (message divs), `:391`/`:394` (`role="dialog"`, `div.close-modal`), `:601`/`:608` (CDN scripts); `dist/js/scripts.js:46-50` (nav-link click closes the toggler) |

Prior attempt: an earlier attempt at this same issue sits on the unmerged remote branch `origin/factory/issue-96` (same target and commit). I did not copy any of its verdicts; this run's trail was produced from scratch in this session. The two were not compared line by line, so no "replication" claim is made.

## 2. How the lens was activated (AC-2), and what is weaker than it sounds

1. `target-constitution.md` §2 declares type `website` and characteristic `must-be-accessible`; its active-lens table lists `accessibility` as the only active lens. `lenses/accessibility.md` frontmatter says `triggers: [must-be-accessible]`, `phases: [specify, design, act, review, qa]`.
2. The reviewer and the QA tester were each told **where the constitution is** and to read its active lenses and the lens's `phases` themselves. Neither prompt named the lens, its path or an item. Both agents' reports state the derivation (`review.md` §1, `qa.md` §1). That closes the by-hand gap of the api run for the two agent dispatches.
3. **Weaker than it sounds:** the dispatcher was this orchestrator following the skills' steps, not a person typing `/peer-review` or `/demo-day`; the constitution was written by the same orchestrator, not through `/charter` or a Facilitator and not by the maintainers, so `must-be-accessible` is an assumed profile; I told the agents to read the constitution (the skills would normally do the passing, `review.md` F2).
4. `website` would also activate `seo` and `ux`; both were turned off in the constitution on purpose, to keep the run to one lens (an override, recorded there).
5. Specify, Design and Act were not exercised by their agents; the spec's NFR-1/NFR-2 were written by me in the shape lens §1 asks for.

## 3. Did the lens fire without a trace request? (AC-3; n=1, no rate)

The reviewer prompt held no per-item trace request, no checklist table and no lens item names. It did hold the spec and plan, which name the accessibility lens, so it knew the lens was in play.

| Lens Review item (`accessibility.md` §4) | Surfaced in `review.md` | Live-confirmed in QA |
|---|---|---|
| No keyboard trap | yes, as a "not a trap" ruling on F7 | QA: Escape/Close Project exit works (PASS); B5 shows a Tab-out-of-modal oddity |
| Focus on modal/route change | yes, F6 (mobile nav) | B3 confirmed: `activeElement` is `BODY#page-top` after a nav-link Enter |
| ARIA correctness not re-checked here | not applied literally: the reviewer also filed baseline findings (F8, F9, F10, F11) | n/a |

Observed: both focus/trap Review items surfaced in one session of one model; the reviewer also went beyond the lens's narrow remit. **This is one observation, not a rate.** QA's prompt likewise named no lens items; it ran all three `accessibility.md` §5 checks.

## 4. Live lens checks (AC-4), from `qa.md`

| Check | Verdict | Observable |
|---|---|---|
| §5.1 keyboard-only, portfolio modal | PASS | Enter opens `#portfolioModal1`; Escape closes it; `activeElement` returns to the trigger link |
| §5.1 keyboard-only, navbar | toggler PASS; focus after link FAIL | `aria-expanded` false to true by Enter; after Enter on "Contact", `activeElement` = `BODY#page-top` (B3) |
| §5.1 keyboard-only, contact form | `⚠ not-verified-live` | fields were filled and submitted by script, not by keys |
| §5.2 measured contrast | PASS as a measurement; pairs FAIL | team muted text 4.45:1 on `#f8f9fa`; hero CTA 1.55:1; placeholder 1.26:1; body text and footer pass (4.69, 15.43) |
| §5.3 live region fires | FAIL | 0 `[aria-live],[role=status],[role=alert]` before and after submit; messages shown by class toggle only |
| NFR-1 / NFR-2 | FAIL / FAIL | see `qa.md` §2; unlabelled form fields (0 `label`), unnamed dialog, no announcement |
| `#submitErrorMessage` state | `⚠ not-verified-live` | could not be triggered |

Review predictions vs. live: F6, F7, F8, F9, F10, F11 confirmed; F12 not exercisable. The network log showed Google Fonts, Font Awesome and jsDelivr all 200, and `sb-forms-latest.js` 302 to `sb-forms-0.4.1.js` — the container's browser did reach the CDNs.

## 5. What is not shown (AC-5)

- **One run, one model, one session per role.** n = 1; no rate; no lens-free control run (a plain review might have found the same defects).
- **Prompt-enforced only.** Nothing in this repo checks that the lens fired; the agents followed Markdown.
- **Profile assumed and dispatch partial** (§2.3): no `/charter`, no maintainer confirmation, orchestrator-run dispatch.
- **Not run:** Specify by a `product-owner`, Design, Act. The target was not changed, nothing was fixed and re-tested.
- **No assistive technology:** no screen reader, zoom/reflow, reduced-motion or touch-device test; keyboard behaviour is Chromium only. Announcement results are DOM semantics.
- **Keyboard-only form completion not performed** (§4).
- **A whole-file snapshot of a 2023 theme, served on localhost**, not a real change and not the live public site. The form behaviour depends on a third-party script as served on 2026-10-28.
- **QA round 2 was a check of files by the orchestrator, not a second browser session** (`qa.md`).

## 6. Failures and misfires (refuted-with-finding)

- **R1 — review F2/F3/F5 are process findings against this run:** the lens paths were resolved by the reviewer on instruction rather than passed by the skill; the installed plugin (`/opt/aspark` v0.14.2) lags this repo (v0.16.0) and lacks `standards/`, so I passed `standards/anti-generosity.md` by hand.
- **R2 — a gate that cannot close on its own:** the skill expects all plan tasks `done` before Review, but T3–T5 are the QA and evidence tasks. T2 was marked done when it finished; the order is a limitation of this run shape, not a fix.
- **R3 — QA could not verify AC-1/AC-5/AC-6 in round 1** because the artifacts did not exist yet; they are closed in round 2 (`qa.md`) from files, not the browser.
- **R4 — QA B9** shows the page's form says "Email is not valid." and "Form submission successful!" together; the cause is a third-party script that logs that the form is not activated. Not asserted further.

## 7. What this does and does not show

It shows: with `accessibility` named in a declared constitution profile and read from there by both agents, the lens's Review slice surfaced its focus items without a trace request, and its QA slice was run live in a real browser against a public MIT page at a pinned commit, giving binary results with observables (three keyboard checks, measured contrast, a live-region FAIL). It does not show a firing rate, superiority over a plain review, an independent profile author, or assistive-technology behaviour. The website half of the lens proof is **partially** closed, not closed.

## 8. Scratch and fences (AC-6)

Scratch: `/tmp/target/startbootstrap-agency` (clone plus the untracked constitution). No script written or committed; the target is unmodified; `lenses/`, `skills/`, `agents/` unchanged.
