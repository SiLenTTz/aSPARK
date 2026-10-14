# Release: campaign-notice-statuses

| | |
|---|---|
| **Phase** | Keep |
| **Owner** | Release Manager (`/go-live`) |
| **Input** | `review.md` (`passed`), `qa.md` (`passed`) |
| **Status** | `preparing` |
| **Version** | v0.14.2 (proposed only, `pr` mode: no tag before merge) |
| **Date** | 2026-10-14 |
| **Commit** | `f7f1120af4fc21b8bde3f5e53a4343e883c4c821` (release commit, local only; merge commit pending) |
| **PR** | pending (not yet opened) |
| **Ticket** | none (no tracker write-back) |

**Handoff**
- **Status:** `preparing` (mirrors the header table; authoritative for `Status` and `Version`).
- **Summary:** patch release v0.14.2: `/campaign` now puts one fixed "Other campaign" line in its report for another live campaign and stays silent on non-live ones. Prompt material, passed QA by the user's waiver, not by the floors.
- **Open:** `4 outstanding` - push, open PR, user merge, tag on the merge commit (owner: user, self-review via PR); see §3.
- **Binding ruling:** §3 Release Actions and the KEEP GATE below carry the final ruling.
- **On conflict:** the numbered body below wins for everything except `Status`/`Version`; log the mismatch as a finding at the next `/go-live` and proceed.

## 1. Pre-Flight Checks

- [x] `review.md` status is `passed` (round 2; REVIEW GATE complete; F1 waived by the user, F8-F12 fixed r2)
- [x] `qa.md` status is `passed` (round 2) as a **user-ruled pass with caveats**: the user waived Majors B2/B3 on 2026-10-14. The suppression floors (AC-2.1 strict, 2.2, 2.3, 2.6) were NOT met; the floors are not claimed. Method per the project's declared standing QA method (constitution §8, no browser surface); QA ran and recorded every `AC-`/`NFR-` ID
- [x] Test suite: no code; `git ls-files '*.py'` empty; `claude plugin validate .` passes (1 pre-existing warning: root `CLAUDE.md`), re-run after the release commit
- [x] Build: N/A (Markdown plugin); `.claude-plugin/plugin.json` is valid JSON, version 0.14.2
- [x] Working tree clean apart from the untracked `.spark/.guard/` (untracked, not in the diff)
- [x] Branch: `git fetch` done; merge-base = `origin/main` = `60a5699`, 0 behind / 9 ahead before the release commit; `git merge-tree --write-tree origin/main HEAD` clean (exit 0)
- [x] Fence (AC-3.2) on `origin/main...HEAD` before the release commit: only `skills/campaign/SKILL.md`, `campaigns/README.md` (two figures) and `.spark/campaign-notice-statuses/`. The release commit adds only `.claude-plugin/plugin.json` (outside the fence by plan D6; user confirmed at plan approval Q2)

## 2. Changelog

### Added
- `/campaign` now puts one line in its report, "Other campaign: <name>, Status <Status>.", for each other campaign that is `approved` or `running`, so you can spot a clash before approving the new draft. If a status cannot be read it says only "Status not readable".

### Changed
- `/campaign` is told to say nothing about other campaigns that are `draft`, `halted`, `complete` or `abandoned`, and not to say that none is live.
- The README gains the new QA counts for the draft state and the kind check.

### Fixed
- Nothing is claimed as fixed. This is prompt material: likely, never certain. Measured in QA round 2 with real sessions:
  - Other-campaign notice: 9 of 10 on the registered ten (5 of 10 before the rewording), 17 of 20 pooled with ten more.
  - `complete` and `abandoned` instances: never named in 6 of 6; strict 5 of 6, because one reply carried an absence line.
  - `draft` and `halted` instances: strict 3 of 6, so they are still sometimes announced or noted.
  - The notice may appear for an instance that is not live. Several or malformed instances were not scored.
- The suppression part passed QA only by the user's waiver of two Major findings; its floors were missed.

## 3. Release Actions

| Action | Result |
|---|---|
| Version bump & tag | Done locally: release commit `f7f1120`, `.claude-plugin/plugin.json` 0.14.1 -> 0.14.2 (patch: behaviour change to prompt material, no new surface; only file carrying the version). No tag (pr mode): tag v0.14.2 on the merge commit after merge, with the user's go. |
| PR / merge | PENDING, needs the user's go; repo `a-lottes/aSPARK` (from `origin` `git@github.com:a-lottes/aSPARK.git`): 1. `git push -u origin fix/campaign-notice-statuses` 2. `gh pr create --repo a-lottes/aSPARK --base main --head fix/campaign-notice-statuses --title "chore(release): v0.14.2, /campaign notice by Status" --body-file <PR body drafted from §2, naming the user-ruled QA pass>` 3. user self-reviews and merges 4. record pass: tag v0.14.2 on the merge commit, append `.spark/releases.md`, fill the Commit/PR rows with executed results. PR-open, validate-passing and approver-requested not yet established. |
| Deploy | N/A - handed-off, no deploy |
| Post-release smoke check | N/A - handed-off, no deploy (after merge: `plugin.json` version 0.14.2, `skills/campaign/SKILL.md` carries the "Other campaign:" line) |
| Rollback | Before merge: close the PR, delete the branch. After merge: `git revert -m 1 <merge-sha>` via a new PR (restores 0.14.1 and the old step 2 wording). If a tag was set: `git push --delete origin v0.14.2`. |

## 4. Learnings (Keep!)

- **What went well:** reviewer and QA re-scored every reply from raw sessions and agreed exactly; the fence, 75-line budget and protected rules held; the interrupted batch was disclosed and rerun, not counted; refuted-with-finding and the waiver were recorded as such.
- **What we'd do differently:** development rates overstated QA's (9 of 10 vs 5 of 10 for the same wording), so a development matrix is not a forecast. One wording that fixed one pole (notice vs suppression) moved the other across four wordings; consider a structural approach, not another wording.
- **Patterns worth reusing (proposed for CLAUDE.md, not edited):**
  - A rule to "say" something at a mid-run step is mostly never spoken, because the agent emits text only in its closing report; put a required line where the output is written.
  - Do not forecast QA from a development matrix; state the expected gap up front.
  - When one wording trades one failure pole for another, stop rewording and change the structure.

---

## ✅ KEEP GATE

- [x] All pre-flight checks passed at release time
- [x] Changelog written in user-facing language
- [ ] Release actions executed and verified - NOT YET: pushed/PR/merge/tag await the user's go; rollback path written
- [x] Learnings recorded
- [x] Line budget respected: Ist ~80 / Soll ~100 (excluding HTML comments), self-reported
- [ ] Status set to `handed-off` - stays `preparing`; outstanding: push, PR, merge, tag v0.14.2, owner: user (self-review via PR)
