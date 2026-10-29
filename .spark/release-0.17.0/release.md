# Release: release-0.17.0

<!-- aspark-template: aspark/0.1.0 -->

| | |
|---|---|
| **Phase** | Keep |
| **Owner** | Release Manager (`/go-live`) |
| **Input** | the `review.md` and `qa.md` of each feature listed in §2 |
| **Status** | `handed-off` |
| **Version** | v0.17.0 |
| **Date** | 2026-10-29 |
| **Commit** | release commit on `factory/release-0.17.0`, on top of `6ecf293` (PR #122, carry-over of #44, #91, #105); tag `v0.17.0` is made on the merge commit after the merge |
| **PR** | https://github.com/a-lottes/aSPARK/pull/123 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status` and `Version`).
- **Summary:** a bundle release of everything merged since v0.16.0: ticket flow in `/next-steps`, `/backlog`, `/story-time` and `/go-live`, the observability lens, companion install offer, template version marker and four public lens runs.
- **Open:** `3 outstanding` — merge by a person, then tag `v0.17.0`; next-steps-triage QA is `changes-requested` (B1 Major open); the template marker is released before its two consumers read it.
- **Binding ruling:** §3 Release Actions and the KEEP GATE below carry the final ruling.
- **On conflict:** the numbered body below wins for everything except `Status`/`Version`; log the mismatch as a finding at the next `/go-live` and proceed — don't stop on it.

## 1. Pre-Flight Checks

- [x] `review.md` is `passed` for all ten features in §2
- [ ] `qa.md` is `passed` for nine of ten; `next-steps-triage` is `changes-requested` (B1 Major open, see §2 Known limitations). Already merged to `main` in PR #111; disclosed here, not hidden
- [x] Test suite: none by constitution §4; `plugin.json` and `marketplace.json` parse as JSON; `git ls-files '*.py'` empty. `claude plugin validate .` not run (no CLI here)
- [x] Build: N/A, Markdown-only plugin
- [x] Working tree clean before the release edit
- [x] Branch fresh: built on `factory/carry-over` (PR #122), which is `origin/main` (`1cd29cc`) plus the three features that were merged only into `factory/main` (#44, #91, #105). After this release `factory/main` holds nothing that `main` lacks
- [x] Version: `plugin.json` on `main` already reads `0.17.0` (bumped in #120 for the companion offer, no release record yet), so no bump in this release. Minor: new optional capabilities only (constitution §5)
- [x] Diff fence: this report and `.spark/releases.md` only

## 2. Changelog

### Added
- **Observability lens** for api projects (`observability-lens`, #89): a Review lens for logs, metrics, traces and health checks.
- **Companion install offer** (`companion-install-offer`, #45): `/charter` and a fresh `/spark` offer `aspark-graph` and `aspark-guard` once, and record the answer in constitution §10.
- **Template version marker** (`template-version-marker`, #44): every template carries `<!-- aspark-template: aspark/0.1.0 -->`, so consumers can check the version instead of guessing from structure.
- **Ticket triage in `/next-steps`** (`next-steps-triage`, #110): reads open tickets, finds gaps, puts a foundation slice first on new projects.
- **Follow-up tickets** (#113): `/peer-review`, `/demo-day` and `/go-live` propose a ticket for each finding left open.
- **Public lens runs** as worked examples: api lens on spring-petclinic-rest (`api-lens-public-run`, #91), observability lens (`observability-lens-public-run`, #94), website lens on eleventy-base-blog, runs 1 and 2 (`website-lens-public-run`, #96; `website-lens-seo-ux-run`, #105).

### Changed
- **`/backlog` checks existing issues** (`backlog-dedupe`, #114): open and closed issues are compared before filing; items already covered or done are marked, not filed again.
- **`/story-time` asks on the ticket** (`ticket-gate-comment`, #115): the spec-gate question is posted to the issue, and an answer there from someone with write access counts.
- **`/go-live` closes its ticket** (#112): the release PR carries a closing keyword, and the ticket comment states the outcome.
- `agent.yaml` allows `cat`, `sed` and `python3` for the Factory agent (#99).

### Fixed
- Nothing claimed as fixed.

### Known limitations
- `next-steps-triage` QA is `changes-requested`: in 1 session the agent proposed a new foundation slice instead of reusing an open foundation ticket (B1, Major), so the Factory could file a duplicate. AC-5 and AC-6 are partial and user-waived (no `gh`, Factory in another repo).
- The template marker is read by nobody yet: `aspark-graph` and `aspark-guard` still have to assert it. The spec's plan was to hold Core's release until both land; this release ships it earlier, which is safe because the marker is an HTML comment and consumers must tolerate it missing. The ROADMAP item stays under Blocked.
- #112 and #113 have no own `.spark/` trail.
- This is prompt material: a best-effort check, not a guarantee.

## 3. Release Actions

| Action | Result |
|---|---|
| Version bump & tag | No bump: `plugin.json` already reads 0.17.0 on `main` (from #120). No tag yet; tag `v0.17.0` after merge. |
| PR / merge | Prepared on request of the person. Branch `factory/release-0.17.0` pushed, PR opened into `main`. It includes PR #122; a person merges it. After the merge the Factory or a person tags the merge commit `v0.17.0`. |
| Deploy | N/A — handed-off, no deploy |
| Post-release smoke check | N/A — handed-off, no deploy |

Rollback: before merge, close the PR and delete the branch; after merge, `git revert -m 1 <merge-sha>` via a new PR (removes this record and the carry-over of #44, #91 and #105; the other features already sit in `main` and need their own reverts), and delete any tag created.

## 4. Learnings (Keep!)

- **What went well:** the gap between `factory/main` and `main` was found by comparing the branches, and closed in one carry-over PR instead of rebuilding the work.
- **What we'd do differently:** release after each feature lands; seven merges sat on `factory/main` with no release PR, and `plugin.json` was bumped with no record.
- **Patterns worth reusing:** before a bundle release, list each feature's `review.md`/`qa.md` status in the pre-flight; one `changes-requested` is disclosed by name, not averaged away.

---

## ✅ KEEP GATE

- [ ] All pre-flight checks passed at release time — no: next-steps-triage QA open, disclosed above
- [x] Changelog written in user-facing language
- [x] Release actions executed: push and PR done; merge and tag still pending (a person merges)
- [x] Learnings recorded
- [x] Line budget respected: Ist ~85 / Soll ~100 (excluding HTML comments)
- [x] Status set to `handed-off` (release mode `pr`, constitution §7)
