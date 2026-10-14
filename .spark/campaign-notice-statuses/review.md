# Review Report: campaign-notice-statuses

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | The diff `origin/main...HEAD` (base `60a5699`), round 2 focus `1c9a70d..HEAD` (65eca05 wording D2, a536eb9 QA round 2, 241feb7 T10); `.spark/campaign-notice-statuses/plan.md` |
| **Status** | `passed` |
| **Round** | 2 |
| **Date** | 2026-10-14 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Verdict:** Passed, with five new Minors. Wording D2 sits at `SKILL.md:31-34`, 75 lines, added lines max 88 characters, frontmatter and every protected rule intact, fence clean. My re-score of QA round 2 from the raw replies matches QA exactly: notice 9 of 10, suppression 5 of 6 strict / 6 of 6 lenient, draft/halted 3 of 6 strict / 4 of 6 lenient. The draft/halted and absence misses are waived Majors (QA B2/B3), recorded as waivers, not as passes. The development figure for D2 draft/halted is overstated in `evidence.md` (F8).
- **Open:** `0 open`. F8-F12 (Minor) `fixed r2` by the owner in `/increment` fix-mode, artifact and README-figure corrections only. Blockers: none. Majors: none open. F1 waived (re-derived, still holds). F2, F3 accepted. F4 fixed r1. F5, F6, F7 fixed r2 (F7 completed by the reviewer).
- **Binding ruling:** §6 Verdict and the gate checklist below — the only binding location; there is no other round to point to
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

- **Reviewed:** `git diff 1c9a70d..HEAD` (SKILL.md step 2 `:31-34`, `campaigns/README.md:22,48`, `evidence.md` fix-round and T10 sections, `plan.md` T10 and Deviation 5, new `qa.md`), with `SKILL.md` re-read whole. Unchanged parts since round 1 are cited from round 1 below, not re-reviewed.
- **Re-derived from raw sessions.** Triggers: (a) this round verifies the fixes to B1/F1, and (b) AC-1.1, 2.1-2.3 and 2.6 are Must criteria. I read every `reply.txt` of `qa3/` R1-R10, S1-S6 and D1-D6 myself. I checked each fixture's Status cell and that each session wrote `campaign.md`. I confirmed R3 is the hostile prompt (`scen.sh:17`). From the transcripts under `~/.claude/projects/` I confirmed wording D2 ("put one line") was loaded in R3, S2, D2, D3 and D6, and that all 22 sessions had model `claude-sonnet-5-5`, a `success` result and no limit marker.
- **Interrupted batch is not counted.** `qa3/limited/` holds 33 streams (K-*, E*, M1, Ma-Mj, R11-R20). All 33 contain "hit your session limit", and none of them is R1-R10, S or D. Each of their rerun `stream.jsonl` has 0 limit markers. I also re-scored the fix-round development sets (`cns/` Z, V, K, J, L, M; D2 transcripts 23:35-23:37Z). I spot-checked wording D (23:31-23:33Z), including V3's double. The 19:21Z batch is the disclosed limit-hit run.
- **Static checks run myself:** `wc -l` = 75. The diff awk printed nothing. The added lines are 84/88/86/88/88/87 characters, so the real maximum is 88, not "under 91" (F12). Frontmatter `:1-9` is identical to `origin/main`. Pre-existing `:55/:62/:73` are 91/92/91, unchanged. `claude plugin validate .` passes with 1 warning (root `CLAUDE.md`, pre-existing). `git ls-files '*.py'` is empty. `.spark/.guard/` is untracked.
- **Lens:** `library`. The public surface is unchanged: name, argument and frontmatter. The output gains one fixed line, "Other campaign: <name>, Status <Status>.", which is additive. The version stays 0.14.1 in the diff, and the 0.14.2 bump belongs to `/go-live`.
- **Tool:** `aspark-graph` not queried (Markdown is not indexed).
- **Cited, not re-derived:** step 3 trim and the protected rules from round 1 §2 D3. `1c9a70d..HEAD` touches only `:31-34`, and I re-read `:27-30,35-46,57-69` unchanged. NFR-7 negatives, SN and X sets are from `qa.md:36,43,46`.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1-T9 | ✅ | Unchanged since round 1 apart from the D2 rewrite. Deviation 5 (`plan.md:226`) records the fix round, wordings D and D2, and the dropped "Then go on" |
| T4 (re-read) | ✅ | Six verdicts in D2: approved/running `:31-32`; draft/halted/complete/abandoned `:32-33` "Say nothing"; absence `:33` "nor that none is live"; unreadable `:33-34` |
| T5 fix round | ⚠️ | Rates for D are confirmed. For D2, draft/halted strict is overstated (F8) |
| T10 | ⚠️ | `done`. Figures only, "Best effort, not a guarantee" kept, no word saying fixed or guaranteed, stale `:48` sentence untouched (its `-`/`+` text is identical). But 47 of 47 pools old-plugin drafts, and the registered figure required by the DoD is missing (F10). The placement at `:22` is ambiguous (F9) |
| Deviations 1-5 | ✅ | Item 5 present. Its flow claim checks out: 22 of 22 R/S/D sessions wrote the draft with no "Then go on" |

## 3. Findings

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Major | `skills/campaign/SKILL.md:32`; `evidence.md:95` | **AC-2.3 sits below its pre-declared floor.** `draft`/`halted` was 4 of 6 strict on wording C (E2, G1 named it as "no clash"), against a 6-of-6 floor on a Must AC. **Fix:** a user ruling, either a waiver to QA or a fix round. **r2 re-derivation:** the waiver's premise ("QA measures fresh, a miss is refuted-with-finding") still holds. After the NFR-2 fix round, QA round 2 measured 3 of 6 strict on D2, and `qa.md:41,61` records that as refuted-with-finding under the user's B2 waiver | **waived by the user** (see §6) |
| F2 | Minor | `evidence.md:130`; `SKILL.md:31` | **A5, several live instances: 0 of 2 announced** in its one development session. Unscored, n = 1. **Fix:** QA reruns and discloses it, and the README must not imply coverage. r2: QA E5 named both live instances and not `complete` (re-read, 2 of 2). The README makes no multi-instance claim | accepted by the user (unscored, spec §6) |
| F3 | Minor | `evidence.md:91`; X5 transcript | **The notice sits at its floor with no slack, and its miss is the hostile goal.** **Fix:** QA reports the hostile row separately. r2: unchanged, R3 hostile is the only registered miss (9 of 10), and R13 also missed in the pooled set | accepted by the user (no slack, disclosed) |
| F4 | Minor | `evidence.md:82` | Over-claim "So B fixed most of B2". **Fix (reviewer, r1):** now reads "So B moved the suppression set to 5 of 6 (A: 1 of 6)" | fixed r1 |
| F5 | Minor | `SKILL.md:33-34`; `plan.md:224` | D1(e)'s unknown-Status case was dropped without a record. **Fix:** record it in Deviation 3. r2: `plan.md:224` names A6 (`runing`) | fixed r2 |
| F6 | Minor | `evidence.md:97` | No per-session rows, tools or model, and the read-only Bash calls were invisible. **Fix:** add a per-session record. r2: `:97` gives the model, the tool counts (71 read-only Bash calls) and the strict result per session id | fixed r2 |
| F7 | Minor | `plan.md:218`; `spec.md:39` | Artifact state mismatches. r2: the A7 resolution is fixed. `plan.md:218` was only half fixed: the same sentence still said "T10 `blocked`". **Fix (reviewer, r2):** now "T10 `todo`" | fixed r2 |
| F8 | Minor | `evidence.md:174,176`; `cns/L1/reply.txt:20` | **The D2 development draft/halted figure is overstated.** L1 says "Other campaign: none live, so there is nothing to report.", which is an absence line. That is a strict miss under C7/AC-2.6, so strict is **3 of 6** (lenient 4 of 6), not 4 of 6. `:176` "D2: 9, 5, 4" should read "9, 5, 3", so D2 was worse than C in development, not equal. **Why it matters:** NFR-2 honesty. The error favours the shipped wording, though QA's fresh 3 of 6 binds and the README does not carry the figure. **Fix:** `/increment` corrects `:174` (add L1 to the misses) and `:176` | `fixed r2` |
| F9 | Minor | `campaigns/README.md:22` | "7 of 7 at the `campaign-notice-statuses` QA" sits inside the parenthesis that qualifies `campaign-start`'s "24 of 24 at round 2", and 17 + 7 = 24, which invites reading it as part of the 24. **Why it matters:** a public rate is misattributable. **Fix:** move it out of the parenthesis, e.g. "...(17 of 17 with the goal on the command line); 7 of 7 at the `campaign-notice-statuses` QA; a miss said..." | `fixed r2` |
| F10 | Minor | `campaigns/README.md:48`; `plan.md:125` | "47 of 47 at the `campaign-notice-statuses` QA" includes 9 drafts written by the `origin/main` comparison plugin (Ma-Mj minus Mg, re-counted). T10's DoD says "registered and pooled n of m stated separately", and the registered 22 of 22 (`qa.md:47,74`) is not stated. **Why it matters:** an undocumented deviation from T10, and the pooled figure is not purely the shipped skill. **Fix:** state "22 of 22 registered, 47 of 47 pooled" or record a deviation | `fixed r2` |
| F11 | Minor | `plan.md:13` | The Handoff Summary still says the rewrite "puts the notice here and not in the report". D2 now puts it "in the report" (`SKILL.md:32`). Artifact wording, capped at Minor. **Fix:** `/increment` updates the Summary to D2 | `fixed r2` |
| F12 | Minor | `qa.md:15,49` | The QA Handoff lists "B5 `open`", but `qa.md:64` says "accepted by the user". The NFR-1 row says "no added line over 91 chars", but the bar is 90 and the real maximum is 88. Artifact wording, capped at Minor. **Fix:** the QA owner aligns B5 and states "max 88 ≤ 90" | `fixed r2` |

## 4. Requirements Traceability

| Spec ID | Implemented at | Verdict |
|---|---|---|
| AC-1.1 | `SKILL.md:31-32` | ⚠️ best effort: QA r2 9 of 10 strict (reviewer re-score 9 of 10), at the floor (F3) r2 |
| AC-1.2 | `SKILL.md:31` "one line" | ✅ 0 doubles in R1-R10 (re-read) r2 |
| AC-2.1 / 2.2 | `SKILL.md:32-33` | ❌ set strict 5 of 6 (S2 absence line), lenient 6 of 6; QA B3 waived r2 |
| AC-2.3 | `SKILL.md:32-33` | ❌ strict 3 of 6, lenient 4 of 6 (D2, D6 named; D3 absence line); F1/B2 waived r2 |
| AC-2.4 | `SKILL.md:31-34` | ✅ six verdicts by reading |
| AC-2.5 | `SKILL.md:29-30` | ✅ 2 of 2 (cited, `qa.md:43`) |
| AC-2.6 | `SKILL.md:33` "nor that none is live" | ❌ 2 of 12 absence lines (S2, D3); B3 waived r2 |
| AC-3.1 | `SKILL.md:27-30,38-46,57-69` | ✅ kind check 7 of 7 (re-checked: key named, porcelain empty, commits 1/1), directory 4/4, draft shape 47/47 (re-counted) r2 |
| AC-3.2 | fence | ✅ re-checked after T10: SKILL.md, README (figures), feature artifacts only r2 |
| NFR-1 | `SKILL.md` | ✅ 75 lines, added lines max 88 |
| NFR-2 | `evidence.md`, `qa.md`, README | ⚠️ floors missed and waived, disclosed. Dev figure overstated (F8) r2 |
| NFR-3 / 4 / 5 | `SKILL.md:1-9`; M1 | ✅ surface unchanged, rule in step 2 |
| NFR-6 / 7 | fence, validate | ✅ |

## 5. What Was Checked

- [x] Correctness: D2 gives every Status a verdict. "Then go on" is gone, and 22 of 22 sessions still went on to write the draft. The wording does not invite the notice for non-live instances: `:32-33` says "Say nothing of one at `draft`, `halted`...", explicitly and without ambiguity. So B2 is **model non-compliance, not a wording defect**. One caveat: all four QA r2 suppression misses (S2, D2, D3, D6) reuse the template prefix "Other campaign:", so the fixed line plausibly raises the leak's salience. That is unproven, since C missed at a similar rate in a different shape.
- [x] Non-functional: NFR-1 to NFR-7 and the `library` lens, as in §4
- [x] Error handling: the step 3 refusal is unchanged
- [x] Security: hostile R3/R13 complied with the user's text, and nothing beyond the draft was written
- [x] Tests: 22 registered QA sessions and 22 D2 development sessions re-scored. The limited batch was verified as uncounted
- [x] Honesty of artifacts: `qa.md:8,61-62,84,88` records B2/B3 as a user waiver with its reason, while the AC rows `:39-44` stay ❌. The floors are not presented as passed

## 6. Verdict

**Waiver record:** F1 was waived by the user on 2026-10-13. Reason: the development restatement budget was spent, QA measures fresh, and a miss there is refuted-with-finding. QA B2/B3 (Majors) were waived by the user on 2026-10-14 (`qa.md:84`). No Blocker was waived.

**Reviewer's verdict (round 2):** Passed. The fix round did what NFR-2 allows: one rewrite (D2 at `SKILL.md:31-34`), still 75 lines, with added lines at 88 characters or fewer, unchanged frontmatter and every protected rule intact. The fence after T10 is clean, and validate passes. My re-scores from the raw replies agree with QA round 2 exactly: notice **9 of 10** strict and lenient (QA 9/10; R3 hostile missed), suppression **5 of 6 strict, 6 of 6 lenient** (QA same; S2 absence line), and draft/halted **3 of 6 strict, 4 of 6 lenient** (QA same; D2 and D6 named the instance, D3 an absence line). The interrupted batch is not counted. The suppression floors are missed, and `qa.md` records that honestly as the user's waiver, not as a pass. The wording itself is clear, so the leak is model behaviour. The README change is figures only. I found five Minors. F8 is the one that matters: `evidence.md` overstates D2's development draft/halted figure (3 of 6, not 4 of 6). F9 and F10 are README precision issues that `/increment` should fix before `/go-live`, since the README is user-facing. F11 and F12 are artifact wording. I completed F7 myself. None of the five blocks the gate.

---

## ✅ REVIEW GATE

*All boxes checked → `/demo-day` may start. Any box open → back to `/increment`. On
re-review, edit this same checklist in place — never duplicate it as a second gate.*

- [x] No open Blocker findings
- [x] No open Major findings (or explicitly waived by the user, with reason recorded here). F1 waived by the user on 2026-10-13 (reason in §6), re-derived in r2 and still valid. QA B2/B3 waived 2026-10-14
- [x] Every Must AC traces to implementing code; no constitution non-negotiable violated
- [x] All plan deviations documented and accepted. Deviations 1-5 are present. F10's T10 deviation is undocumented, a Minor that does not block
- [x] Test suite runs green: validate passes, and 44 sessions (22 QA r2 plus 22 D2 development) were re-scored by the reviewer. The rate figures are best effort
- [x] Line budget respected: Ist 102 / Soll ~150 (excluding HTML comments). Self-reported, no linter checks this
- [x] Status set to `passed`. Minors F8-F12 are open for `/increment` fix-mode or user acceptance, with F9/F10 ideally before `/go-live`
