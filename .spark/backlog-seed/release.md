# Release: backlog-seed

<!-- aspark-template: aspark/0.1.0 -->

| | |
|---|---|
| **Phase** | Keep |
| **Owner** | Release Manager (`/go-live`) |
| **Input** | `review.md` (`passed`), `qa.md` (`passed`) |
| **Status** | `preparing` |
| **Version** | v0.16.0 (proposed) |
| **Date** | 2026-10-27 |
| **Commit** | `pending` — set after the release commit exists and the tag is made post-merge |
| **PR** | `pending` — opened only on the person's go |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status` and `Version`).
- **Summary:** `/backlog` turns `docs/` into an ordered, user-edited list filed as GitHub issues; proposed v0.16.0.
- **Open:** `3 outstanding` — go to publish; filing (AC-4.x) never run live; the `0.15.0` bump from adversarial-gates has no release record.
- **Binding ruling:** §3 Release Actions and the KEEP GATE below carry the final ruling.
- **On conflict:** the numbered body below wins for everything except `Status`/`Version`; log the mismatch as a finding at the next `/go-live` and proceed — don't stop on it.

## 1. Pre-Flight Checks

- [x] `review.md` status is `passed` (round 4)
- [x] `qa.md` status is `passed` (round 1); QA ran by the project's standing method (constitution §8), not a per-release override
- [x] Test suite: none by constitution §4; `plugin.json` and `marketplace.json` parse as JSON; `git ls-files '*.py'` empty. `claude plugin validate .` not run (no CLI here), as already waived at review
- [x] Build: N/A, Markdown-only plugin
- [x] Working tree clean before the release edit
- [x] Branch fresh: `factory/release` was 11 commits behind `origin/main` and carried commits not in main. Rebuilt off `origin/main` (`be9af0f`); the `/backlog` work (PR #82) is already in main
- [x] Diff fence: `.claude-plugin/plugin.json` and this report only

## 2. Changelog

### Added
- `/backlog`: for a repo with documents but no backlog. The Product Owner reads `docs/`, the constitution and `ROADMAP.md`, proposes an ordered list of feature-sized items, each traced to a source section. You edit the list, and on your explicit yes each item is filed as a GitHub issue that `/story-time <n>` can import.

### Changed
- Nothing else. `/next-steps` is unchanged.

### Fixed
- Nothing claimed as fixed.

### Known limitations
- Filing issues (AC-4.1 to 4.3, AC-5.2) was never run live because `gh` was absent in QA; the user waived it. Steps 1 to 6 ran live.
- Minor, accepted: Depends field can hold prose (B1), caller may reword items (B2), no consistent ceremony count in docs (B3), JSON reports skipped on the agent's own call (B4).
- This is prompt material: a best-effort check, not a guarantee.

## 3. Release Actions

| Action | Result |
|---|---|
| Version bump & tag | Proposed 0.15.0 -> 0.16.0 in `plugin.json`, committed locally. Minor: new optional skill (constitution §5). Review F10 said 0.15.0, but main already carries 0.15.0 from adversarial-gates (`0c9144a`, no release record), so 0.16.0 avoids one number naming two surfaces. No tag now; tag `v0.16.0` after merge. |
| PR / merge | Pending the go. Would run: `git push -u origin factory/release`, `gh pr create --base main`. Nothing pushed. |
| Deploy | N/A — handed-off, no deploy |
| Post-release smoke check | N/A — handed-off, no deploy |

Rollback: before merge, close the PR and delete the branch; after merge, `git revert -m 1 <merge-sha>` via a new PR (removes the version bump; `/backlog` itself already sits in main from PR #82, so revert that merge too to remove the skill), and delete any tag created.

## 4. Learnings (Keep!)

- **What went well:** Steps 1 to 6 were run for real in fresh sessions; the filing gap was disclosed and waived, not passed.
- **What we'd do differently:** The release branch drifted from main and a version number was claimed by a parallel feature; check both at Plan.
- **Patterns worth reusing:** Before proposing a version, compare `plugin.json` on `origin/main` with `releases.md`; a bump with no release row means an unreleased feature shares the number.

---

## ✅ KEEP GATE

- [x] All pre-flight checks passed at release time
- [x] Changelog written in user-facing language
- [ ] Release actions executed and verified — pending the go
- [x] Learnings recorded
- [x] Line budget respected: Ist ~75 / Soll ~100 (excluding HTML comments)
- [ ] Status set to `handed-off` — pending; stays `preparing`
