# Release: campaign-draft-polish

| | |
|---|---|
| **Phase** | Keep |
| **Owner** | Release Manager (`/go-live`) |
| **Input** | `review.md` (`passed`), `qa.md` (`passed`) |
| **Status** | `preparing` |
| **Version** | v0.14.1 (proposed only, `pr` mode: no tag before merge) |
| **Date** | 2026-10-12 |
| **Commit** | `<release commit SHA, filled after the user's go>` |
| **PR** | `<url, filled after the user's go>` |
| **Ticket** | none (no tracker write-back) |

**Handoff**
- **Status:** `preparing` (mirrors the header table; authoritative for `Status` and `Version`).
- **Summary:** patch release v0.14.1: `/campaign` drafts now carry a filled title, a fixed closing sentence and a best-effort notice of other live campaigns. All three are prompt material.
- **Open:** `3 outstanding` - push, open PR, user merge/tag (owner: user, self-review via PR); see §3.
- **Binding ruling:** §3 Release Actions and the KEEP GATE below carry the final ruling.
- **On conflict:** the numbered body below wins for everything except `Status`/`Version`; log the mismatch as a finding at the next `/go-live` and proceed.

## 1. Pre-Flight Checks

- [x] `review.md` status is `passed` (round 1, with conditions; the binding condition, T11, verified below)
- [x] `qa.md` status is `passed` (round 1, with caveats; method per the project's declared standing QA method, constitution §8, no browser surface; QA ran and recorded every `AC-`/`NFR-` ID)
- [x] Test suite: no code, no `*.py`; `claude plugin validate .` passes (1 pre-existing warning: root `CLAUDE.md`)
- [x] Build: N/A (Markdown plugin); `skills/campaign/SKILL.md` 75 lines, widest line 90 characters
- [x] Working tree clean apart from the untracked `.spark/.guard/` (not tracked, not touched)
- [x] Branch: `git fetch` done, 0 behind / 6 ahead of `origin/main` 771a1e2; `git merge-tree --write-tree origin/main HEAD` clean (exit 0)
- [x] Fence (AC-4.2) on `origin/main...HEAD`: only `skills/campaign/SKILL.md`, `campaigns/README.md` (one line) and `.spark/campaign-draft-polish/`; constitution untouched
- [x] T11 done: the README diff is one inserted figure ("and 38 of 38 at the `campaign-draft-polish` QA"); "Best effort, not a guarantee" kept

## 2. Changelog

### Added
- `/campaign` tells you, once and with its status, when another campaign is `approved` or `running`, so you can spot a clash before approving the new draft. Best effort only.

### Changed
- `/campaign` now ends with one fixed sentence: "This draft is not startable until these are filled and you approve."
- The README states the measured draft-state rate.

### Fixed
- The draft's title is now filled with the campaign name instead of a placeholder (39 of 39 drafts in QA).
- The closing sentence is now exact (40 of 40 sessions in QA; 3 of 6 before).
- These three are prompt material: likely, never certain. Observed in QA with real sessions:
  - Other-campaign notice: 9 of 10 on the registered ten, 17 of 20 pooled with ten more. That is not better than the earlier 6 of 7, so no improvement is claimed for it.
  - Drafts for `draft` and `halted` instances stayed silent (5 of 6 if one neutral line counts as a miss).
- Known open minor gaps:
  - The notice was missed in 3 of 20 sessions.
  - `complete` and `abandoned` instances are still mentioned.
  - One draft dropped the kind's heading.
  - The kind check is not shown on the happy path (also so on the previous release, 0 of 6).
  - QA sessions used tools outside the stated set.

## 3. Release Actions

| Action | Result |
|---|---|
| Version bump & tag | Done locally: `.claude-plugin/plugin.json` 0.14.0 -> 0.14.1 (patch, behaviour fix; no other file carries the version). No tag (pr mode). Precedent: v0.14.0's tag was set pre-merge on the user's explicit ask; user decides. |
| PR / merge | PENDING (needs the user's go), repo `a-lottes/aSPARK` (from `origin`): 1. `git push -u origin fix/campaign-draft-polish` 2. `gh pr create --repo a-lottes/aSPARK --base main --head fix/campaign-draft-polish --title "chore(release): v0.14.1, /campaign draft polish" --body-file <PR body, drafted from §2>` 3. user self-reviews and merges 4. post-merge record pass: tag v0.14.1 on the merge commit, append `.spark/releases.md`, set `released`/`handed-off` fields. |
| Deploy | N/A - handed-off, no deploy |
| Post-release smoke check | N/A - handed-off, no deploy |
| Rollback | Before merge: close the PR, delete the branch. After merge: `git revert -m 1 <merge-sha>` via a new PR (restores 0.14.0 and the old SKILL.md; the only user-visible change is the three prompt wordings). If a tag was set: delete it with `git push --delete origin v0.14.1`. |

## 4. Learnings (Keep!)

- **What went well:** reviewer and QA re-derived rates from raw sessions and caught a miscount (AC-1.2 4 of 6, not 5 of 6); the fence and the 75-line/90-character budget held; baseline runs on `origin/main` showed B4 is not a regression.
- **What we'd do differently:** AC-1.1 passed on the registered ten at the exact floor, yet 17 of 20 pooled, no better than baseline; decide up front how extra sessions count. Name every Status in a suppression rule (F3 gap left `complete`/`abandoned`).
- **Patterns worth reusing (proposed for CLAUDE.md, not edited):**
  - A development rate matrix and QA's pooled extra sessions can disagree; the pre-declared floor on the registered set decides, and the pooled figure is disclosed, not used to claim improvement.
  - Trimming a "redundant" phrase needs a check that no other ban rode on it (review F2, "run or offer nothing").
  - Suppression rules should name every Status, not two (review F3).

---

## ✅ KEEP GATE

- [x] All pre-flight checks passed at release time
- [x] Changelog written in user-facing language
- [ ] Release actions executed and verified - prepared, awaiting go (push, PR, CI, approver, merge pending); rollback path written
- [x] Learnings recorded
- [x] Line budget respected: Ist ~88 / Soll ~100 (excluding HTML comments), self-reported
- [ ] Status set to `handed-off` - pending: after the PR is open, remaining is the user's merge and the v0.14.1 tag (owner: user as self-review approver; outside the Release Manager's control)
