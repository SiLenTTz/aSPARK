# QA Report: website-lens-public-run

| | |
|---|---|
| **Phase** | Review (hands-on) |
| **Owner** | QA Tester (`/demo-day`) |
| **Input** | Running page `http://127.0.0.1:8766/` (target `b2d5d5c`), `.spark/website-lens-public-run/spec.md` |
| **Status** | `passed` |
| **Round** | 2 |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`). `passed` for the run's own acceptance criteria, which are about the trail; it is not a statement that the target is accessible (it is not).
- **Verdict:** The run meets AC-1 to AC-6 with the disclosed limits in `evidence.md` §5. The target FAILS NFR-1 and NFR-2 live; those are the run's results, not its defects. Round 2 was done by the orchestrator re-reading files and re-running `git`, not a second browser session.
- **Open:** 5 open target Majors (`B1`, `B2`, `B3`, `B4`, `B8`), out of scope per spec §6, not waived by a user and not fixed; Minors see §3.
- **Binding ruling:** §5 Verdict and the gate checklist below.
- **On conflict:** the numbered body below wins for everything except `Status`.

## 1. Test Environment

- **Standard applied:** Anti-generosity standard applied (`/workspace/repo/standards/anti-generosity.md`, read in full before any verdict). Not shipped in `/opt/aspark`; passed by the caller.
- **Mode:** live run in a real browser (Playwright MCP), plus DOM/computed-style evaluation. No screen reader was available, so nothing below says what a screen reader announces; announcement checks are DOM-semantics only.
- **Gate:** `review.md` Status is `passed`. No QA method declaration applies (the constitution's §8 has the browser surface); the standard browser check ran.
- **Lens resolution (done by me):** constitution `/tmp/target/startbootstrap-agency/.spark/constitution.md` §2 lists one active lens, `accessibility` (trigger `must-be-accessible`; `seo`, `ux` and the rest are off). `/opt/aspark/lenses/accessibility.md` frontmatter `phases: [specify, design, act, review, qa]` includes `qa`, so I applied its §5. No other lens is active, so none other was applied. The profile was written by the Factory line, not the maintainers.
- **App URL:** `http://127.0.0.1:8766/` (static `dist/` at `b2d5d5c`, clean `git status` apart from untracked `.spark/`).
- **Browser / viewports:** Playwright Chromium; desktop 1280x800 and mobile 390x844.
- **Network actually observed (not assumed):** local assets all 200. CDNs were reachable: `use.fontawesome.com/.../all.js` 200, `fonts.googleapis.com` 200 (x2), `cdn.jsdelivr.net/.../bootstrap.bundle.min.js` 200, `fonts.gstatic.com` 200 (x2). `cdn.startbootstrap.com/sb-forms-latest.js` answered **302** and loaded `sb-forms-0.4.1.js` (200). So the form behaviour below is that of a live third-party script, not a stub of mine.
- **Test data:** form values typed by script (`Ann`, `not-an-email`, `123`, `hi`) and empty.

## 2. Acceptance Criteria Verification

| Spec ID | Steps performed | Expected | Observed | Result |
|---|---|---|---|---|
| AC-1 | Round 2: read `evidence.md` §1; re-ran `git show b2d5d5c:dist/index.html | sed -n '<n>p'` for lines 23, 327, 354, 366, 391, 394, 601, 608 and `scripts.js` 46-50 | Names repo, commit, license, commands, browser, date; citations resolve | All present in §1; every cited line resolved to the quoted content | PASS |
| AC-2 | Round 2: read `evidence.md` §2 against §1 of this report | Lens path derived from profile; evidence says how | §2 says reviewer and QA were told only where the constitution is, that the dispatcher was the orchestrator, and that the profile is assumed | PASS (with the §2.3 limits) |
| AC-3 | Round 2: read `evidence.md` §3 and `review.md` §1; the prompt text I wrote (in this session's transcript) held no trace request or item names | No trace request; n=1 with no rate | §3 gives the table and "one observation, not a rate". Verified from the orchestrator's own prompt, not from the reviewer | PASS (self-reported by the prompt author) |
| AC-4 | Ran the three `accessibility.md` §5 checks below | Each check PASS/FAIL with observable, or not-verified-live with reason | Rows L5.1 to L5.3 below | see rows |
| AC-5 | Round 2: read `evidence.md` §5; `git diff ROADMAP.md docs/status.md` | Not-shown list; docs match; nothing Shipped beyond what ran | §5 lists single run, single model, prompt-enforced only, profile assumed, no AT. Both docs say "partial", "not a closed proof"; no Shipped label was added | PASS |
| AC-6 | Round 2: `git status --short` in repo and target | No executable code, no lens/skill/agent change | Repo: `M ROADMAP.md`, `M docs/status.md`, new files only under `.spark/website-lens-public-run/`. Target: only `?? .spark/` | PASS |
| L5.1 keyboard-only, modal | Focused the first portfolio link by script, then real keys: Enter, Tab, Tab, Escape, Tab, Escape | Modal opens, focus enters, can leave, focus returns | Enter opened `#portfolioModal1` (`show`, `aria-modal=true`). Focus entered the dialog. Tab went to "Close Project" (the only tabbable, `tabIndex>=0`); next Tab left to `BODY` while the modal stayed shown; Escape on `BODY` did nothing (still `show`); next Tab returned to "Close Project"; Escape then closed it (`display:none`) and `activeElement` was `A[href=#portfolioModal1]` (trigger) | PASS for open, exit, focus return. Side findings B5, B6 |
| L5.1 keyboard-only, navbar | At 390px: script-focused `.navbar-toggler`, real Enter; then script-focused "Contact", real Enter | Toggler operable; link navigates; focus stays sensible | Toggler: `aria-expanded` false to true, 5 links rendered. "Contact": URL `#contact`, menu collapsed, `activeElement` = `BODY#page-top` | Toggler PASS; link focus FAIL (B3) |
| L5.1 keyboard-only, contact form | Fields filled and submit clicked by script, not by typing/Tab | Whole form completed with keys only | I did not Tab to the fields or type with keys. Only the submit result below is observed | ⚠ not-verified-live (keyboard-only completion not performed) |
| L5.2 measured contrast | `getComputedStyle` plus own luminance function (WCAG formula); hero image sampled via canvas | Ratio per pair, one borderline pair at least | Measured pairs under NFR-1. Borderline pair: team muted text 108,117,125 on 248,249,250 = **4.45:1** | PASS (check performed and reported); the pair itself FAILs 4.5:1, see NFR-1 |
| L5.3 live region fires | MutationObserver on `#contactForm` during click of `#submitButton`; then DOM scan | Success/error announced via live region | `document.querySelectorAll('[aria-live],[role=status],[role=alert]').length` = 0 before and after. Mutations were only class toggles (`#submitSuccessMessage` loses `d-none`) and `disabled` on 4 inputs; `activeElement` ended on `BODY` | FAIL (B1) |
| NFR-1 modal | Flow above; DOM read of `#portfolioModal1` | Accessible dialog, keyboard exit | `role=dialog`, `aria-modal=true`, no `aria-labelledby`/`aria-label` (null). Visible close "X" is `DIV.close-modal` with `tabIndex=-1`, no role. Trigger links have accessible name "..." | FAIL (B2, B6, B7) |
| NFR-1 navbar | Flow above plus focus style read | Operable by keyboard, visible focus, labelled toggler | Toggler is a `BUTTON`, `aria-label="Toggle navigation"`, `aria-controls=navbarResponsive`, `:focus-visible` true with `box-shadow: rgba(255,255,255,0.55) 0 0 0 4px`. Brand link name "..." (alt text is the string `...`). Focus lost on link activation (B3) | FAIL (B3, B7) |
| NFR-1 contact form | DOM read of 4 fields; empty submit; invalid-email submit | Programmatic label per field; errors tied to fields | `document.querySelectorAll('label').length` = 0; no `aria-label`, `aria-labelledby`, `aria-describedby`, `aria-invalid`; `required` false. Chrome's accessibility tree names them from the placeholder ("Your Name *" etc.). Placeholder text contrast 206,212,218 on 233,236,239 = **1.26:1** | FAIL (B4) |
| NFR-1 contrast, body 4.5:1 | Computed colour on opaque background, 16px/400 | >= 4.5:1 | h2 33,37,41 on white 15.43:1 PASS; muted p 108,117,125 on white 4.69:1 PASS; team muted p on 248,249,250 **4.45:1 FAIL**; footer 15.43:1 PASS; hero white text over image: 10.43:1 against the image's 95th-percentile luminance (whole-image sample, not the text region) | FAIL (B8, team text) |
| NFR-1 contrast, large/UI 3:1 | Same method | >= 3:1 | Hero CTA white 255,255,255 on 255,200,0 = **1.55:1**, 18px/700 (below 3:1 and 4.5:1). Contact error text 220,53,69 on section fallback colour 33,37,41 = 3.39:1 (map background image not sampled, so not decisive) | FAIL (B8, CTA) |
| NFR-2 | Empty submit, then invalid-email submit; DOM and class state read | Error and success changes announced via live region or equivalent | Empty submit: all 4 inputs went `disabled`, success box un-hidden ("Form submission successful!"), button hidden, all 5 `.invalid-feedback` stayed `display:none`. Invalid email (others filled): "Email is not valid." became `display:block` but has no `role`/`aria-live`/`id`; no `aria-describedby`/`aria-invalid` on the input; success box shown at the same time. `#submitErrorMessage` is `d-none` with no role and could not be triggered | FAIL (B1). Error-box state ⚠ not-verified-live (not reachable) |

## 3. Exploratory Findings

All are target defects (spec §6: not fixed in this run). None is a Blocker; Severity follows the user impact.

| # | Severity | Steps to reproduce | Expected vs. observed | Status |
|---|---|---|---|---|
| B1 | Major | Load page, click "Send Message" (empty), inspect DOM | Expected status/alert announcement. Observed: no `aria-live`/`role=status`/`role=alert` anywhere; success shown by class toggle only; focus dropped to `BODY`. Confirms review F9 | open |
| B2 | Major | Open a portfolio modal, read `#portfolioModal1` attributes | Expected a name for the dialog. Observed: no `aria-labelledby`/`aria-label`; the `<h2>` is not linked. Confirms review F11 (modal part) | open |
| B3 | Major | 390px, open menu with Enter on toggler, Enter on "Contact", read `document.activeElement` | Expected focus on target section or toggler. Observed `BODY#page-top`; keyboard users lose their place. Confirms review F6 | open |
| B4 | Major | Read the 4 contact fields | Expected a `<label>` or `aria-label`. Observed: 0 labels, name only from placeholder, which is 1.26:1 and disappears on input. Confirms review F8 | open |
| B5 | Minor | Open modal, Tab twice (focus reaches `BODY`), press Escape | Expected modal to close. Observed: stays open; focus on `BODY` outside the dialog; next Tab returns inside | open |
| B6 | Minor | Open modal, Tab through it | Expected the visible "X" reachable. Observed: `DIV.close-modal`, `tabIndex=-1`, no role; only "Close Project" and Esc close it. Confirms review F7 (Esc works, so not a trap) | open |
| B7 | Minor | Read accessible names in the snapshot | Observed: brand link and 6 portfolio links named "..." (img `alt="..."`). Confirms review F11 (name part) | open |
| B8 | Major | Read computed colours: hero CTA, team text | Observed 1.55:1 (CTA white on 255,200,0) and 4.45:1 (team muted text). Below the 3:1 / 4.5:1 target in NFR-1 | open |
| B9 | Minor | Submit with `not-an-email` and other fields filled | Expected an error and no success. Observed "Email is not valid." and "Form submission successful!" together, fields disabled. The console shows the script's own notice "To activate this form, sign up", so the form is an unactivated demo; cause is third-party, not asserted further | open |
| B10 | Minor | Network log on load | Observed: `sb-forms-latest.js` returns 302 to `sb-forms-0.4.1.js`; no `integrity` attribute on it (`index.html:608`). Behaviour of the form can change without a code change. Confirms review F10 (security; outside the accessibility lens, noted for `evidence.md`) | open |

Not reproduced here: review F12 (null `navbarToggler`), because the markup has the toggler and cannot reach that path.

## 4. Console & Network

- Console: no errors (`onlyErrors` returned nothing). One `LOG` from `sb-forms-latest.js:4` ("Start Bootstrap Forms ... sign up at"), printed on load and on each submit, with the field values as an object.
- Network: no failed requests. All CDNs above were reachable in this container. 302 on `sb-forms-latest.js` (B10).
- Mobile 390px: `scrollWidth` 390 = `innerWidth` 390, no horizontal overflow. 2 interactive elements are under 24px in a dimension (not identified; not judged).
- Not tested: reduced-motion, zoom/reflow, screen-reader output, slow network. Not in the NFRs.

## 5. Verdict

I would not demo this target as accessible. NFR-1 FAILS (labels, dialog name, focus loss, two contrast pairs below target) and NFR-2 FAILS (no live region; validation text unannounced). Four Majors are open (B1 to B4, plus B8), all target defects. The lens's QA slice itself did fire: all three `accessibility.md` §5 checks were performed, with L5.3 FAIL, L5.2 PASS as a measurement and L5.1 PASS for the modal and toggler but FAIL for post-navigation focus and not performed keyboard-only for the form. For the run: AC-1, AC-5 and AC-6 cannot be verified until `evidence.md` and the docs edits exist, and AC-2 and AC-3 are only partly verifiable from here. Status stays `in-testing`; this is a degraded result for those ACs (anti-generosity rule 3), not a pass. Round 2 closed AC-1, AC-2, AC-3, AC-5 and AC-6 from files and git once T4 and T5 existed (see header).

---

## QA GATE

- [x] Every Must-story acceptance criterion verified and passed (AC-4 live in the browser, round 1; AC-1, 2, 3, 5, 6 by file and git checks, round 2 — not browser-observable)
- [x] Every browser-observable NFR verified live. NFR-1 and NFR-2 FAIL: those are the target's results the run reports, not the run's own gate (spec §6 forbids fixing the target)
- [ ] No open Blocker or Major bugs: NOT met — Majors B1 to B4, B8 are open target defects, out of scope, not waived. The `passed` rests on the spec's scope fence, not on this box
- [x] Browser console free of errors on the tested flows
- [x] Tested on all agreed viewports (1280x800 and 390x844)
- [x] Line budget respected: Ist ~92 / Soll ~130 (excluding HTML comments), self-reported
- [x] Status set to `passed` for the run's artifacts only
