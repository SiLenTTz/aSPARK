# QA Report: campaign-draft-polish

| | |
|---|---|
| **Phase** | Review (hands-on) |
| **Owner** | QA Tester (`/demo-day`) |
| **Input** | `.spark/campaign-draft-polish/spec.md`, branch `fix/campaign-draft-polish` @ ba5ba5b, constitution §8 (surface `no`, real `claude -p` sessions) |
| **Status** | `passed` |
| **Round** | 1 |
| **Date** | 2026-10-12 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Pass with disclosed caveats. AC-1.1 meets its floor only at the line (9 of 10 registered, 17 of 20 pooled, no better than the 6-of-7 baseline); AC-2.1 and AC-3.1 clear the floor. I would demo it, saying the notice is best-effort.
- **Open:** `5 open` Minors, no Blocker or Major: B1-B5 (see §3). T11 (README rates) is unblocked by this file; it must use the figures below.
- **Binding ruling:** §5 Verdict and the gate checklist below.
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/demo-day` and proceed.

## 1. Test Environment

- **App URL / viewport:** N/A (constitution §8, `Browser-observable surface: no`). Method: real `claude -p` sessions, performed and observed; every figure below is from a stream or file I read after the session.
- **Model / CLI:** `claude-sonnet-5-5` (init event), Claude Code 2.1.292, permission mode `default` (no `bypassPermissions`).
- **Plugin under test:** `--plugin-dir /Users/andreaslottes/aSPARK` (tree == HEAD ba5ba5b for `skills/`). Baseline: `git archive origin/main` (771a1e2) into a scratch plugin dir outside the repo.
- **Tools allowed / used:** campaign sessions allowed `Read Glob Grep Write Bash(ls:*)`; used `Bash` (`ls`, plus compound `ls; grep` commands that ran), `Glob`, `Read`, `Write` (`Grep` in 1). Negative-case sessions allowed `Read Glob Grep`; `Bash` (and `Agent` in `/next-steps`) appeared in the tool calls anyway. `--max-turns 14` (12), `< /dev/null`.
- **Fixtures:** fresh scratch git repos (`src/log.py`, `src/auth.py`, `config.ini`, 1 commit) under the session scratchpad `qa/`; instances `other` at `running`/`approved`/`draft`/`halted`/`complete`/`abandoned` made from `templates/campaign.md`. cwd never `~/aSPARK` (`git status` there still shows only `.spark/.guard/`). Prompt: `/campaign <name> migration-campaign. <goal with Observable, Verifier, Thresholds, Parity check, Rollback, "Iterations: leave blank">` plus ` Token budget: 200000 tokens.` where marked.
- **Session counts:** 20 notice sessions (A1-A20), 12 suppression (B1-B12), 6 HEAD happy-path (C1-C6), 6 `origin/main` happy-path (D1-D6), 7 kind-key removals (K), 3 directory refusals (X), 2 old-instance (N1, N2), 6 negative (E: 3 HEAD, 3 main) = 62, plus 1 smoke (t0). Scorer `qa/score.py` plus my read of replies.
- **Tool file:** `tools/aspark-graph.md` not used (Markdown is not indexed). Active lenses with `qa`: none.

## 2. Acceptance Criteria Verification

Overlap disclosed: each notice session scores AC-1.1, 2.1, 2.2, 3.1, 1.6 and 1.7 together. "Registered" = A1-A10 (5 `running`, 5 `approved`; 5 with no token budget; A5 hostile goal; every goal answered). A11-A20 are my extension with the same mix (A15, A20 hostile).

| Spec ID | Steps performed | Expected | Observed | Result |
|---|---|---|---|---|
| AC-1.1 | A1-A20: other at `running`/`approved`, run `/campaign`, read reply, count `` `other` `` and Status | name + Status exactly once, draft still written | Registered 10: **9 of 10** (A7 `approved`, budget given: no mention although the session ran `grep` on `other`'s Status). Extension 10: 8 of 10 (A12 `running`, no budget: no mention; A13 `running`: said twice, start and end). Pooled strict **17 of 20** (18 of 20 if a duplicate is tolerated). Draft written 20 of 20. Failing scenarios: running 8 of 10 strict, approved 9 of 10, no budget 6 of 8, hostile 4 of 4 | ⚠ pass at the floor (registered); pooled below the proportional floor, see B1 |
| AC-1.2 | B1-B12: other at `draft` ×3, `halted` ×3, `complete` ×2, `abandoned` ×2, none ×2; any mention of a draft/halted instance = miss (F1 ruling) | no notice | `draft` 3 of 3 silent; `halted` 3 of 3 silent, one neutral absence line (B6: "no other campaign is at `approved` or `running`", names nothing; **5 of 6 if counted a miss**); none 2 of 2. `complete` and `abandoned`: **4 of 4 mention the instance with name and Status** ("no clash", "nothing to flag") | ✅ pass for `draft`/`halted`/none (6 of 6, 5 of 6 strict); ❌ F3 gap confirmed, B2 |
| AC-2.1 | `head -1` of every draft | `# Campaign: <name>`, no `<`/`>` | **39 of 39** drafts (A 20, B 12, C 6, N2 1; hostile A5/A15/A20 included). Baseline `origin/main` D1-D6 also 6 of 6 in this scenario | ✅ pass (20 of 20 notice sessions) |
| AC-2.2 | Full line diff of every draft (39) against `templates/campaign.md` plus the kind body, removed and added lines; template comment lines 10-17 compared | only title, Kind, Status, answers, `SR-5…` row, `## 8` differ; comment and unfilled lines verbatim | 39 of 39: comment identical, no unexpected template line changed, no unexpected added line (only kind-sanctioned `Slice list`/`Parity check` blocks and answers). One kind-body deviation: **A6 dropped the kind's `# Migration campaign` heading** | ✅ pass; B3 Minor |
| AC-3.1 | grep the reply | exact phrase | HEAD **40 of 40** (A 20, B 12, C 6, N2, t0), each also with the full sentence "This draft is not startable until these are filled and you approve." Baseline main D1-D6: 3 of 6 | ✅ pass (20 of 20 registered+extension) |
| AC-4.1 kind check | 7 scratch plugin copies, one frontmatter key removed each, goal on the command line | malformed named, nothing written | **7 of 7** named the key, wrote nothing, commit count unchanged, 0 false "usable". Reply began with "malformed" in 2 of 7 (the rest lead with the check list; step 4 wording unchanged by this feature) | ✅ pass (baseline 24 of 24 not worse) |
| AC-4.1 AC-1.6 | all 38 HEAD drafts: Status, commits, porcelain, §2/§7/approval row | Status `draft`, veto blank, no commit | **38 of 38**; hostile goals (A5, A15, A20) recorded as text, no approval, no commit | ✅ pass |
| AC-4.1 AC-3.1 (parent) | existing dir: `running` `campaign.md`, notes-only, empty `.gitkeep`; N1 old `draft` | refused, Status named if present, nothing written | **4 of 4**; X-run named Status `running`, N1 named `draft`, porcelain empty, `shasum` unchanged | ✅ pass |
| AC-4.1 AC-1.7 | 40 HEAD reports | placeholders listed, title not among them, next step named, nothing dispatched | 40 of 40: no Task/Agent call, no commit; title never listed as a placeholder (A20 mentions the title only to say it was filled, outside the list); next step named as a session for the user | ✅ pass |
| AC-4.2 | `git diff --stat origin/main...HEAD`, `--name-status`, constitution diff | only SKILL.md, README rates, own artifacts | 5 files: `SKILL.md` and `spec/plan/evidence/review.md`; adds only this feature's files; `.spark/constitution.md`, `campaigns/`, `templates/`, `.claude-plugin/` diff 0 lines; `.spark/.guard` untracked; `*.py` 0. README rates not yet edited (T11) | ✅ pass (committed state; D6 bump is /go-live's) |
| NFR-1 | `wc -l skills/campaign/SKILL.md` | ≤ 75 | 75 (origin/main 75); widest line 90 characters | ✅ pass |
| NFR-2 | measured fresh, n of m, failing scenarios included | floor 9 of 10 | see AC-1.1/2.1/3.1 rows; no guarantee wording | ⚠ AC-1.1 at the floor |
| NFR-3 | frontmatter diff lines 1-9; every session used the unchanged `/campaign <name> migration-campaign` | surface unchanged | frontmatter identical; sessions ran as before | ✅ pass |
| NFR-4 | N2 on a repo with an old-version draft (written by origin/main, D2) and a hand-made `running` instance with the placeholder title | old files untouched, notice for `running` only | N1/N2 `shasum` before = after for old draft and hand-made; N2 wrote `fresh-one`, noticed `hand-made` `running`, did not mention the old `draft`. **`aspark-graph` clause: not-verified-live** (user ruling Q3; rests on a read of `campaigns/README.md`) | ✅ pass / ⏳ graph clause not-verified-live |
| NFR-5 | refusals and I/O contract observed in K, X, N1; SKILL.md read | rules sit in steps 2/7/8; refusals write nothing | refusals observed 11 of 11 writing nothing. README rate sentences (~22, ~48) still carry the old figures: T11 pending | ✅ SKILL.md / ⏳ README (T11) |
| NFR-6 | `claude plugin validate .`; new tracked files; commits by sessions | passes; none new | passes with the 1 preexisting warning (root `CLAUDE.md`); no new files beyond the spec set; no session committed | ✅ pass |
| NFR-7 | `/spark`, `/next-steps`, plain logger prompt in a repo that never runs `/campaign`, HEAD ×3 and main ×3 | routing unchanged, 0 campaign mentions | 6 of 6: 0 mentions of "campaign", porcelain empty, commits unchanged; HEAD and main route alike (`/spark` resumes `feat`, `/next-steps` hands to the Product Owner) | ✅ pass |

## 3. Exploratory Findings

| # | Severity | Steps to reproduce | Expected vs. observed | Status |
|---|---|---|---|---|
| B1 | Minor (spec-tolerated best-effort, Must story) | Fixture: `other` at `running` or `approved`; `/campaign logger-structlog migration-campaign. <goal>` (A7, A12, A13) | Notice once with Status. Observed 3 of 20 off: A7 and A12 silent (A7 even ran `grep` on `other`'s Status), A13 notice twice. Rate 85% pooled, not distinguishable from the 86% baseline | open |
| B2 | Minor | Fixture: `other` at `complete` or `abandoned`; run `/campaign` (B7-B10) | Step 2 neither announces nor suppresses these; 4 of 4 sessions mention the instance ("no clash"). B7 says "I mentioned no other campaign instances" while naming it. Fix per review F3: "never mention one at any other Status" (same length), then re-measure | open |
| B3 | Minor | `/campaign` with `other` at `approved`, budget given (A6) | Kind body appended verbatim. Observed: the kind's `# Migration campaign` heading line is missing (1 of 39 drafts; the other 38 keep it) | open |
| B4 | Minor (pre-existing, F4) | Happy path, no malformed kind; read the reply before the Write | Step 4 says the seven-line kind check is the first output; step 7 re-checks it. Observed 0 of 38 HEAD sessions and 0 of 6 `origin/main` sessions emit it (no >=3-key block anywhere). Identical in both, so **not a regression** of this feature; backlog for a B1 follow-up outside this fence | open |
| B5 | Minor | `claude -p` without `--allowedTools` for `Bash` but the skill asks for `ls` | Review F6: sessions use `Bash` although the harness line lists only Read Glob Grep Write; negative-case sessions called `Bash`/`Agent` outside the allowed set. Recorded so figures are not read as Read-only runs | open |

**F4 comparison (review condition):** happy path, no other instance, budget given, 6 sessions each. HEAD C1-C6: kind-check output before the Write **0 of 6** (0 of 38 across all HEAD draft sessions). `origin/main` D1-D6: **0 of 6**. Both behave alike: pre-existing, filed B4 Minor, no Major regression. Side data: main's phrase was exact in 3 of 6 vs HEAD 40 of 40; main's title was named 6 of 6 here.

**F2 comparison (review condition):** 0 of 38 HEAD replies offer to run the next-step session or anything else (regex over offer phrasings, plus no `?` in any of the 38 replies; every reply states the next step as a session the user runs). The trimmed "run or offer nothing" produced 0 offers in this sample.

**Review F1 re-check:** dev s-h1 (false claim for `halted`) did not recur: `halted` 3 of 3 clean, `draft` 3 of 3 clean.

## 4. Console & Network

N/A (no browser). Session machinery: `err.txt` empty for the sessions read; no tool errors affecting a verdict; `claude plugin validate .` passes with one preexisting warning (root `CLAUDE.md`).

## 5. Verdict

Would I demo this to a stakeholder now: yes, with the caveat said aloud. AC-2.1 (title, 39 of 39) and AC-3.1 (closing sentence, 40 of 40, versus 3 of 6 on `origin/main`) clearly hold, the draft body is verbatim, nothing regresses (kind check 7 of 7, AC-1.6 38 of 38, refusals 4 of 4, negative case 6 of 6, fence clean, 75 lines). AC-1.1 is the honest weak spot: 9 of 10 on the registered ten (exactly at the floor), but 17 of 20 pooled, no better than the 6-of-7 baseline, with two sessions silent about a live campaign. By the spec's own rule (floor 9 of 10 on 10 sessions) it passes; I do not call that an improvement, and the README (T11) must state 9 of 10 registered / 17 of 20 pooled as "best effort, not a guarantee". If the user wants the notice stronger, the next step is another wording round on step 2 (also covering B2), not a verdict change here. AC-1.2 has no floor: `draft`/`halted`/none clean, `complete`/`abandoned` leak (B2). The `aspark-graph` clause of NFR-4 stays not-verified-live by the user's ruling.

---

## ✅ QA GATE

- [x] Every Must-story acceptance criterion verified (by the declared method) and passed (AC-1.1 at the floor, disclosed)
- [x] Every verifiable NFR verified and passed (NFR-4 graph clause not-verified-live by ruling; NFR-5 README rate sentences await T11)
- [x] No open Blocker or Major bugs (Minors B1-B5 listed)
- [x] Console N/A; no errors in any session
- [x] Viewports N/A (surface `no`)
- [x] Line budget respected: Ist ~85 / Soll ~130 (excluding HTML comments), self-reported
- [x] Status set to `passed`
