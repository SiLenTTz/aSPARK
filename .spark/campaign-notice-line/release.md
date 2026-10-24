# Release: campaign-notice-line

| | |
|---|---|
| **Phase** | Keep |
| **Owner** | Release Manager (`/go-live`) |
| **Input** | `review.md` (`passed`), `qa.md` (`passed`) |
| **Status** | `handed-off` |
| **Version** | v0.14.3 (proposed only; no tag before merge, `pr` mode) |
| **Date** | 2026-10-24 |
| **Commit** | b2c1921 (release commit) |
| **PR** | https://github.com/a-lottes/aSPARK/pull/80 |

**Handoff**
- **Status:** `handed-off` (authoritative for `Status` and `Version`)
- **Summary:** `/campaign` now lists every other non-live campaign in its closing report; patch bump 0.14.2 to 0.14.3, PR #80 opened, awaiting review and merge.
- **Open:** `outstanding` — merge, real tag v0.14.3 and approval, owned by `a-lottes` (approver), outside our control
- **Binding ruling:** §3 Release Actions and the KEEP GATE below carry the final ruling.
- **On conflict:** the numbered body below wins for everything except `Status`/`Version`; log the mismatch as a finding at the next `/go-live` and proceed — don't stop on it.

## 1. Pre-Flight Checks

- [x] `review.md` status is `passed` (Handoff read, REVIEW GATE 7/7 checked; round 2)
- [x] `qa.md` status is `passed` (QA GATE 7/7 checked; round 1). Per constitution §8, QA is performed by the project's declared method (hands-on against the installed plugin) as a standing project fact; `qa.md` records every `AC-`/`NFR-` ID
- [x] Test suite: none by constitution §4; the bar is `claude plugin validate .` = `✔ Validation passed with warnings` (one pre-existing warning: `CLAUDE.md` at plugin root is not loaded as context); `git ls-files '*.py'` empty
- [x] Build: N/A, Markdown-only plugin, nothing to build
- [x] Working tree: only untracked `.spark/.guard/` (0 tracked files, never staged, does not ship)
- [x] Branch fresh: merge-base with `origin/main` = `4966076` = `origin/main` after fetch
- [x] Diff fence: `skills/campaign/SKILL.md`, `campaigns/README.md`, `.spark/campaign-notice-line/*`, plus `.claude-plugin/plugin.json`; `SKILL.md` 75 lines

## 2. Changelog

### Added
- Nothing new to start or run; the change below reverses an earlier choice.

### Changed
- When you run `/campaign`, the closing report now also lists other campaigns in the project that are not live (draft, halted, complete, abandoned), each with its Status. Before, only live ones were mentioned.
- If a campaign's Status cannot be read, the report says so instead of guessing.
- This is a best-effort check by the assistant, not a guarantee. Fresh test runs: notice 9 of 10, listing 6 of 6, no-other-campaigns case 3 of 3, malformed-kind check 7 of 7, refusal of a directory 4 of 4, refusal of an existing name 2 of 2. The notice set sits exactly on its floor, with no slack. Earlier development figures are not a forecast.

### Fixed
- Nothing claimed as fixed.

### Known limitations
- A Status outside the six known ones (e.g. `runing`) is printed as written and never treated as live, rather than reported as unreadable (3 of 3 sessions). Deferred to a follow-up feature, 2026-10-21.
- `Running` with a capital R is printed as "Running, live". Deferred to the same follow-up.
- If the goal itself says not to mention other campaigns, the line is omitted (2 of 2 sessions). Accepted as by design.
- The malformed-kind check held 6 of 7 in development (review F2) and 7 of 7 at QA.

## 3. Release Actions

| Action | Result |
|---|---|
| Version bump & tag | Proposed v0.14.3 (patch: wording of one closing-report line, no new capability or interface). `.claude-plugin/plugin.json` bumped locally; `marketplace.json` carries no version. No tag in `pr` mode; real tag at/after merge, outside our control. |
| PR / merge | Executed 2026-10-24: `git push -u origin feat/campaign-notice-line` (staleness re-checked: merge-base = origin/main = `4966076`), `gh pr create --base main` opened https://github.com/a-lottes/aSPARK/pull/80 (read-only: `gh pr view` OPEN, base main). No reviewer requested (self-review by `a-lottes`). CI: none exists. Not merged, no tag. |
| Deploy | N/A — handed-off, no deploy |
| Post-release smoke check | N/A — handed-off, no deploy |

Rollback: before merge, close the PR and delete the branch; after merge, `git revert -m 1 <merge-sha>` via a new PR (restores `SKILL.md`, README and 0.14.2), and delete any tag created.

## 4. Learnings (Keep!)

- **What went well:** Rates were re-measured fresh at QA and the README carries QA figures, not development ones; B1-B3 were disclosed and ruled instead of reworded away.
- **What we'd do differently:** A floor with zero slack (notice 9 of 10) means any rewording risks the gate; decide the slack at Plan. F1's wording fix after demo-day cost a round.
- **Patterns worth reusing:** Record a reversal of a predecessor's ruling (C4) in spec and README together; keep `.spark/.guard/` untracked and stage by path, never `git add -A`.

---

## ✅ KEEP GATE

- [x] All pre-flight checks passed at release time
- [x] Changelog written in user-facing language
- [x] Release actions executed and verified: push and PR #80 open (PR-open established by `gh pr view`); approver not formally requested (self-review), no CI exists; rollback path written
- [x] Learnings recorded
- [x] Line budget respected: Ist       76 / Soll ~100 (excluding HTML comments)
- [x] Status set to `handed-off`: merge, real tag and approval outstanding, owned by `a-lottes` (approver), outside our control
