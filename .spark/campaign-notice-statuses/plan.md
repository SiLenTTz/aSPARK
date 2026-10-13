# Plan: campaign-notice-statuses

| | |
|---|---|
| **Phase** | Plan |
| **Owner** | Engineering Manager (`/sprint-plan`) |
| **Input** | `.spark/campaign-notice-statuses/spec.md` (must be `approved`) |
| **Status** | `approved` |
| **Date** | 2026-10-12 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Summary:** One in-place rewrite of the other-instance sentence in `skills/campaign/SKILL.md` step 2. It gives an explicit verdict for each of the six Status values, puts the notice here and not in the report, forbids any absence statement, and allows at most one "Status not readable" line. Net ≤ 75 lines and every changed line ≤ 90 characters, paid for by trims from a named list or the task stops for the user. The skeleton (T3) proves the rewrite end to end. Rates are fresh `claude -p` n of m, strict and lenient side by side. README figures wait for QA (T10).
- **Open:** `1 task not done`: T10 (README figures, waits for QA). Q1 and Q2 answered at approval.
- **Binding ruling:** §3 Task Breakdown for current task status; a plan revision after review/QA findings updates §1/§3 in place, never a new section
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Architecture Decision

- **Context:**
  - **Stack and runner** come from `constitution.md §9`. *Stack & entry points:* `/campaign` names no agent and runs in the main session. *Test practice:* there is no suite; the bar is validate plus a dry run, negative case first. `skills/campaign/SKILL.md` is re-read from source under exception (a), because this feature edits exactly that file. It is 75 lines (the spec's cap, C5). The rule sits at `:31-33`: "For every other instance at `approved` or `running`, tell the user its name and Status once, clash or not; never mention one at `draft` or `halted`; then go on. Read no constitution; a repo with no `.spark/` works the same."
  - **B2 cause.** The sentence gives a verdict for four of the six values in `templates/campaign.md:5`. `complete` and `abandoned` are left to inference, and 4 of 4 sessions inferred "mention" (qa.md B2). One said "I mentioned no other campaign instances" while naming one, which is an absence statement wrapped around a notice.
  - **B1 cause, as far as the data shows.** Silence in A7/A12 and a double notice in A13. There are no instructions about *where* in the reply the notice goes, so it can drift to the report or be repeated there. Silence clusters in `running`, no-budget and goal-answered runs (spec A3).
  - **Absence lines already occur.** "No other campaign instances exist", "none needed a mention" (`campaign-draft-polish/evidence.md:111-112,120`). AC-2.6 now forbids them whenever another instance exists, so the rule has to say so in so many words.
  - **Width.** The previous loop let 3 changed lines through at 91-92 characters. NFR-1 counts the diff's *added* lines, so the awk check has to strip the leading `+` and run on the diff, not just on the file (D3).
  - **Best-effort (CLAUDE.md "Prompt material makes a check likely, never certain"; spec NFR-2):** AC-1.1, 1.2, 2.1, 2.2, 2.3 and 2.6. Deterministic: AC-2.4 (read), AC-2.5 (one refusal run, still agent behaviour, so it gets n of m), NFR-1 and AC-3.2.
- **Decision:**
  1. **D1 Enumerate all six at step 2, with one catch-all for the unknown (AC-2.4, NFR-5).** The sentence at `:31-33` is replaced in place. No new step, no table, no change to frontmatter `:1-9`, the argument, the output or the Never list's bans (NFR-3). The target meaning has six parts, and each is required:
     - (a) **Announce:** for each other instance at `approved` or `running`, its name **and** Status, once, clash or not.
     - (b) **Where:** here only, not repeated in the step 8 report (AC-1.2).
     - (c) **Silence by name:** `draft`, `halted`, `complete` and `abandoned` are never mentioned.
     - (d) **No absence statement:** no line saying that no live one exists (AC-2.6). This is stricter than the spec, which allows such a line when there is no other instance at all; "allowed, not required" is met.
     - (e) **Unknown or unreadable Status (A6/A7):** at most one line "Status not readable", and never treated as a clash.
     - (f) **Kept as is:** "then go on", "Read no constitution; a repo with no `.spark/` works the same", and the self-name refusal before it (AC-2.5: the suppression covers *other* instances only, so the word "other" stays).

     Candidate wording (indent 3; every line measured at ≤ 90 characters by hand, and T3 re-measures it with awk):
     ```
        For each other instance, by Status: `approved` or `running`, tell the user its name
        and Status once, clash or not, and not again in the report; `draft`, `halted`,
        `complete` or `abandoned`, never mention it, nor that no live one exists; a Status
        you cannot read or do not know: at most one line "Status not readable", no clash.
        Then go on. Read no constitution; a repo with no `.spark/` works the same.
     ```
     That is 5 lines in place of 3, so **+2**. `/increment` may change words, and may shorten the text to +1, but must keep all six parts and the step.
  2. **D2 Walking skeleton = the rewrite end to end (T3).** In four live sessions, two that must announce and two that must stay silent, the one edit proves the harness, the line accounting and both halves of the rule before the full sets run.
  3. **D3 Line and width budget (NFR-1, C5).**
     - **Lines:** `wc -l` ≤ 75 after every task. The +1/+2 is paid only by trimming a clause whose rule is still stated elsewhere, in this order:
       - (i) step 3 `:37` "invent no structure or tracker", already stated at `:19` "no tracker, no structure of your own";
       - (ii) Never list `:74` "write a second file", already at `:19` "no second file";
       - (iii) a reflow of one paragraph to recover the freed characters.
     - **Never trimmed:** the kind check (`:38-46`, `:58`), the directory check (`:27-30`), the title rule (`:60-61`) and the closing sentence (`:66`). These are the AC-3.1 regression surfaces.
     - **Width:** `git diff -U0 origin/main -- skills/campaign/SKILL.md | grep '^+[^+]' | cut -c2- | awk 'length>90'` prints nothing. A reflowed line counts as changed. Lines that are already longer and are left untouched are recorded in T1, not fixed.
     - **If ≤ 75 cannot be met without dropping a rule:** the task is set `blocked` and goes to the user. No implementer ruling.
  4. **D4 Harness = `campaign-draft-polish` QA's, made comparable (spec A3).**
     - **Session:** `claude -p` with `--model sonnet`, `--plugin-dir <worktree>`, `--allowedTools "Read Glob Grep Write"`, `--max-turns 12`, `--output-format stream-json --verbose`, and `< /dev/null`.
     - **Repo:** a fresh scratch git repo per session under `<scratchpad>/cns/<id>/` (scratchpad = `/private/tmp/claude-501/-Users-andreaslottes-aSPARK/7c365ac8-8576-47b5-84c0-f822f84973a7/scratchpad`). It holds one commit with `src/log.py`, `src/auth.py` and `config.ini`.
     - **Helpers:** `dp/h.sh` and `dp/score.py` are *copied* to `cns/` and adapted there. `dp/` stays untouched as the previous loop's evidence. The helpers stay untracked (constitution §3).
     - **Never:** cwd in `~/aSPARK`, or `bypassPermissions`.
     - **Recorded per session:** the model (from the init event), `claude --version` and the tools actually called (qa.md B5).
     - **Fixture instance name:** `payments-cutover`, not `other`. "other" is ordinary English and inflates the regex hits. The name change is disclosed; Status, budget and goal mix stay as in A3.
     - **"The reply"** means all assistant text in the stream. The regex flags candidate lines and a human read of each reply decides; both are recorded.
  5. **D5 README figures only after QA (C9, T10).** T10 runs in `/increment` fix-mode once `qa.md` holds fresh n of m, and copies those figures from it. No agent estimates a rate.
  6. **D6 Release belongs to `/go-live`; the patch bump sits outside the AC-3.2 fence (precedent: `campaign-draft-polish` D6).**
     - **The bump:** `/go-live` bumps `.claude-plugin/plugin.json` `0.14.1` → `0.14.2` as the release commit. No task here edits it.
     - **How AC-3.2 is read:** its "`.claude-plugin/` untouched" covers `origin/main...<last pre-release commit>`. Review and QA measure the fence on that range.
     - **Needs the user:** this reading is carried over from the earlier ruling, so the user confirms it again at plan approval (§5 Q2). The spec is not patched.
- **Alternatives considered:**
  | Alternative | Why rejected |
  |---|---|
  | Allowlist with a catch-all: "only `approved`/`running`; never mention one at any other Status" (qa.md B2's proposal, same length) | It fails AC-2.4: four values are left to inference, which is exactly B2's failure mode. It is cheaper by about one line, but the spec chose completeness over brevity |
  | Extend the blocklist to four values with no catch-all | It meets AC-2.4 but leaves a typo or unreadable Status (A6/A7) and the absence line (AC-2.6) undefined. The catch-all and the absence clause cost about one line together |
  | A six-row table or bullet list of Status → verdict inside step 2 | Clearest to read, but about +7 lines against a hard 75 cap, so it would force trims into protected rules. A table inside a numbered step also breaks the file's prose-step style |
  | Move the notice to step 8's report, or to a new step after the interview | The spec's NFR-5 places the rule in step 2, where the instance list is read (CLAUDE.md "Put a rule in the file the agent actually reads"). A step-2 mention plus a report mention is the A13 double (AC-1.2) |
  | Put the suppression in the Never list ("mention a non-live instance") | That is not where the agent acts. It also splits one rule across two places, which is what produced B2 |
  | Add a template comment listing which Status values are live | Out of scope (spec §6). It also changes protected surface (NFR-3) |
  | Phrase the notice as an explicit first output ("before anything else, say…") | Step 4's "first output" kind check was emitted 0 of 38 (qa.md B4), so the evidence does not support position wording. Kept as a fallback for T5's single restatement only |
- **Consequences:**
  - **Easier:** every Status has a verdict that can be read off one place. The scorer can tell a strict miss (double notice, absence line) from a lenient one.
  - **Harder:**
    - The file sits at the cap again, so every future rule must be paid for.
    - A stronger suppression may lower the notice rate (spec A4). T5 and QA measure both halves together.
    - The no-absence clause forbids something the model does naturally, so the 6 of 6 floor is fragile (R4).
  - **Nothing is enforced.** The rule stays an instruction.

## 2. Affected Components

- **Edited:**
  - `skills/campaign/SKILL.md`: step 2 `:31-33`, plus D3 trims (i)-(iii).
  - After QA (T10), `campaigns/README.md`, rate figures only (C9).
- **Feature artifacts:** `.spark/campaign-notice-statuses/plan.md` and `evidence.md`; later `review.md`, `qa.md` and `release.md`.
- **Release commit (`/go-live`, D6):** `.claude-plugin/plugin.json` `0.14.1` → `0.14.2`.
- **Read, unchanged:**
  - `templates/campaign.md` (the six values at `:5`);
  - `campaigns/migration-campaign.md`;
  - `.spark/constitution.md`;
  - the previous loop's `qa.md` and `evidence.md`, for the comparison scenarios only (rates are never carried, NFR-2).
- **Untouched (AC-3.2, NFR-6):** every other `skills/*`, `agents/`, `templates/`, `campaigns/*.md` kinds, `README.md`, `docs/` and the constitution. The untracked `.spark/.guard/` must never be committed.
- **New dependencies, services or patterns:** none. Fixtures, plugin copies and helpers are untracked scratch. Their text and outputs are quoted in `evidence.md`.
- **Blast radius: not queried, scoped by hand.** No tool file was passed. Every `files:` path is Markdown, and `aspark-graph` indexes source only (precedent `campaign-draft-polish/plan.md:83`). An empty answer would mean "not indexed", not "nothing at risk". Scope comes from the reads cited in §1.
- **Existing README figures (read for T10):**
  - `campaigns/README.md:22` and `:48` hold the kind-check, draft-state, refusal and existing-instance figures.
  - **No line states a rate for the other-instance notice.** `grep -n notice` hits only `:69` and `:79`, and neither is a rate. See §5 Q1.

## 3. Task Breakdown

A **performed step** is a D4 session with its prompt and reply quoted in `evidence.md`, followed by the observed `git status --porcelain`. Reading `SKILL.md` closes only AC-2.4 and NFR-1/3/5 (constitution §8). Development figures are labelled *development* and are never QA's.

**Scoring** applies to every task.
- **AC-1.1 strict:** exactly one statement in the reply names `payments-cutover` together with its Status, and the draft is written.
- **AC-1.1 lenient:** at least one such statement.
- **AC-2.1-2.3 strict:** the instance and its Status are never named, there is no absence statement, and the draft is written.
- **AC-2.1-2.3 lenient:** the instance is never named; an absence line is tolerated and counted.

| # | Task | Story | Covers (AC / NFR) | Depends on | Status | Definition of Done |
|---|---|---|---|---|---|---|
| T1 | Branch freshness and baseline record, before the first commit | US-3 | NFR-1, NFR-6 | – | `done` | `evidence.md` records the following. After `git fetch origin`, `git merge-base HEAD origin/main` equals `git rev-parse origin/main`; if not, stop and rebase or cut a fresh branch (CLAUDE.md). Porcelain shows only `.spark/.guard/` untracked. `SKILL.md` baseline: `wc -l` (75), `wc -w`, and `awk 'length>90{print FNR": "length}'` over the whole file, listing the pre-existing long lines. `git ls-files '*.py'` is empty. `claude --version`. `claude plugin validate .` passes — files: .spark/campaign-notice-statuses/evidence.md |
| T2 | Negative case first, baseline and old-instance fixture, all on the unchanged tree | US-3 | NFR-7, NFR-4, AC-2.1, AC-2.2 | T1 | `done` | With `SKILL.md` unchanged. **(a) Negative case:** a scratch repo with a constitution, one mid-loop feature and no `.spark/campaigns/`; `/spark`, `/next-steps` and a plain prompt each recorded (routing, questions), with porcelain empty. **(b) Baseline, disclosed only and never scored against a floor:** 4 D4 sessions on the old wording, other instance at `complete`, `abandoned`, `running` (no budget) and `approved`, each scored strict and lenient. **(c) Fixture repo:** an old-version draft written by the unchanged `/campaign` plus a hand-made `running` instance, committed, with `shasum` recorded — files: .spark/campaign-notice-statuses/evidence.md |
| T3 | **Walking skeleton: D1 rewrite of step 2, end to end** | US-1, US-2 | AC-1.1, AC-1.2, AC-2.1, AC-2.2, NFR-1, NFR-5 | T2 | `done` | **Edit:** step 2 carries all six D1 parts (a)-(f) at `:31` onward; each D3 trim is listed in `evidence.md` together with where its rule still lives. `wc -l` ≤ 75. D3's diff awk prints nothing. **Sessions:** four D4 runs, goal given up front: other at `running` (no budget), `approved`, `complete` and `abandoned`. The first two are strict-clean on AC-1.1; the last two are strict-clean on AC-2.1/2.2; all four write the draft (porcelain: only the new `campaign.md`). Any miss → at most one restatement at step 2 within D3, then rerun all four — files: skills/campaign/SKILL.md, .spark/campaign-notice-statuses/evidence.md |
| T4 | Static audit: six verdicts, size, width, surface | US-2, US-3 | AC-2.4, NFR-1, NFR-3, NFR-5 | T3 | `done` | `evidence.md` holds three tables. **Six-verdict table:** one row per Status value (all six), with its verdict (announce / silent) and the `SKILL.md` line quoted, and no row resting on inference. **Contract table:** input, output, never-written list, each refusal, and the step 2 self-name refusal (AC-2.5), each with its line. **Pre-existing long-line table:** unchanged or reflowed to ≤ 90. Also recorded: `wc -l` ≤ 75; D3's diff awk prints nothing; `git diff origin/main -- skills/campaign/SKILL.md` leaves `:1-9` unchanged; validate passes. If > 75 lines or a rule is gone: `blocked`, ask the user, change nothing further — files: .spark/campaign-notice-statuses/evidence.md |
| T5 | Development rates on the registered shapes | US-1, US-2 | AC-1.1, AC-1.2, AC-2.1, AC-2.2, AC-2.3, AC-2.6, NFR-2 | T4 | `done` | One row per session: scenario, number of notice lines, absence line yes/no, strict, lenient, porcelain, tools used. **Notice (10):** `running` ×5 and `approved` ×5; 5 with no token budget; 1 hostile goal; every goal answered. **Suppression (6):** `complete` ×3 and `abandoned` ×3. **Draft/halted (6):** `draft` ×3 and `halted` ×3. Results: strict and lenient n of m per set. Below 9 of 10, or below 6 of 6: one development restatement at step 2 within D3 (a position phrasing is allowed here; §1 last row), then that set is rerun fresh, with both rows kept. A restatement that needs > 75 lines → `blocked` for the user — files: skills/campaign/SKILL.md, .spark/campaign-notice-statuses/evidence.md |
| T6 | Edge cases (disclosed, unscored) and self-name | US-1, US-2 | AC-2.5, AC-2.6 | T5 | `done` | **One session each, disclosed:** (A5) two live instances plus one `complete`, where each live one gets one line and the `complete` one is silent; (A6) Status `runing`; (A7a) a directory with no `campaign.md`; (A7b) the template copied with its Status cell unfilled; and no other instance at all, where an absence line is allowed (AC-2.6). For A6/A7, record whether there is at most one "Status not readable" line and no clash claim. **AC-2.5:** `/campaign` with a name that already exists at `complete`, and one at `abandoned`. Each is refused, naming the directory and its Status, with `shasum` unchanged; n of 2 — files: .spark/campaign-notice-statuses/evidence.md |
| T7 | Regression AC-3.1 | US-3 | AC-3.1 | T6 | `done` | **Kind check:** 7 D4 sessions on scratch plugin copies with one key removed each; each names the key and writes nothing (n of 7). **Directory refusal:** 4 sessions (existing `campaign.md` at `running`, one at `draft`, a directory with only `notes.md`, an empty directory), each refused with nothing changed (n of 4). **Over every draft from T3, T5 and T6:** Status `draft`, veto record blank, no commit (`git rev-list --count` unchanged), `head -1` = `# Campaign: <name>`, and `grep -F "not startable until these are filled and you approve"` hits. Each figure is n of m, compared with 7/7, 38/38, 4/4, 39/39 and 40/40; any shortfall is a finding — files: .spark/campaign-notice-statuses/evidence.md |
| T8 | Compatibility dry run on old instances | US-3 | NFR-4 | T7 | `done` | On T2(c)'s fixture repo with the changed skill: a new draft gives the notice for the hand-made `running` instance once and does not mention the old `draft`; the self-name for the old draft is refused, naming its Status; `shasum` before = after for both old files — files: .spark/campaign-notice-statuses/evidence.md |
| T9 | Post-change negative rerun and diff fence (before the release commit) | US-3 | NFR-7, NFR-6, AC-3.2 | T8 | `done` | T2(a)'s three sessions rerun and compared by routing: no difference, no campaign mention. On `origin/main...HEAD`: `git diff --stat` lists only `skills/campaign/SKILL.md` and `.spark/campaign-notice-statuses/*`; `--diff-filter=A` adds only this feature's artifacts; the constitution, `templates/`, `campaigns/*.md` and `.claude-plugin/` diffs are empty; `.spark/.guard/` is untracked; `git ls-files '*.py'` is empty; validate passes — files: .spark/campaign-notice-statuses/evidence.md |
| T10 | README rate figures from QA's n of m (after `/demo-day`, fix-mode) | US-3 | AC-3.2, NFR-2, NFR-5 | T9, `/demo-day` qa.md | `todo` | Done only once `qa.md` holds fresh rates. Only **existing** figures in `campaigns/README.md:22`/`:48` that QA re-measured are updated, each matching its `qa.md` row; adding a notice-rate figure needs the user's answer to §5 Q1. `git diff -U0 campaigns/README.md` shows figures only (plus Q1's clause if ruled), registered and pooled n of m stated separately. "Best effort, not a guarantee" remains, and no word says fixed, improved, guaranteed or always. AC-3.2 is then re-checked; the next gate re-derives it — files: campaigns/README.md |

**Release pointer (not a task):** `/go-live` owns Keep.
- PR into `main`, local `claude plugin validate .`, terminal status `handed-off`.
- The release commit is the D6 bump `0.14.1` → `0.14.2`.
- Pre-flight re-checks branch staleness and confirms that `.spark/.guard/` is absent from what ships (constitution §3/§6).

## 4. Test Strategy

Prompt material has no suite (constitution §4).
- **Structural (unit analogue):** `wc -l`, D3's diff awk, `grep -F`, `head -1`, `shasum`, `git diff --stat` and `claude plugin validate .`, each with its output quoted.
- **Live (integration analogue):** D4 sessions. The negative case runs first (T2a) on the unchanged tree and again last (T9).
- **Cheap baseline (T2b):** 4 sessions on the old wording, disclosed only, for context. It is not a comparison base, because rates are fresh (NFR-2).

**Stories:**
- **US-1 (Must):** T3 skeleton, then T5 (10), then QA.
- **US-2 (Must):**
  - AC-2.4 by reading (T4);
  - AC-2.1/2.2/2.6 in T3 and T5 (6);
  - AC-2.3 in T5 (3 + 3);
  - AC-2.5 in T6;
  - A5/A6/A7 disclosed in T6.
- **US-3 (Must):** T7 for AC-3.1, T8 for NFR-4, T9 and T10 for the AC-3.2 fence, T1 and T4 for NFR-1.

**At `/demo-day` (QA's figures are the ones that ship).** The QA Tester runs fresh D4 sessions against the installed plugin on the PR head.

**Pre-declared sets.** Floors apply to these sets only, strict count; lenient is reported beside it.

| Set | Sessions | Floor | ACs |
|---|---|---|---|
| Negative | 3 (`/spark`, `/next-steps`, plain prompt), **run first** | no difference | NFR-7 |
| Registered notice | 10: `running` ×5, `approved` ×5; 5 with no budget; 1 hostile; all goals answered | 9 of 10 | AC-1.1, AC-1.2 |
| Suppression | 6: `complete` ×3, `abandoned` ×3 | 6 of 6 | AC-2.1, AC-2.2, AC-2.6 |
| Draft/halted | 6: `draft` ×3, `halted` ×3 | 6 of 6 | AC-2.3, AC-2.6 |
| Self-name | 2 (`complete`, `abandoned`) | n of 2, every miss a finding | AC-2.5 |
| Regression | as T7 | no worse than parent (§3 T7) | AC-3.1 |
| Extras | A5, A6, A7a/b, no-other; any additional notice runs | disclosed, unscored | — |

**Rules for the figures:**
- Extras never offset a registered miss, never lower a floor and never support a claim of improvement.
- A 9 of 10 means "not worse than 9 of 10 last time" (A2).
- Below a floor: one `/increment` fix round, then the set is rerun fresh. Still below → `refuted-with-finding` and the user decides (NFR-2, CLAUDE.md).
- Rates are reported as "n of m, strict (lenient n of m)", never as "fixed".

**Re-derive at each gate:** a D3 trim made to fund the rule can weaken another rule. The AC-3.1 figures, the six-verdict table and the line/width figures are re-derived after every fix pass, not carried forward (CLAUDE.md "A fix to one finding can silently invalidate another finding's waiver").

## 5. Risks & Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| R1: +2 lines cannot be funded without dropping a rule | NFR-1 fails, or a rule is lost silently | D3's trim order (duplicates only). `blocked` for the user, no implementer ruling (C5) |
| R2: The stronger suppression lowers the notice rate (A4) | AC-1.1 below 9 of 10 | Both halves are measured in the same tasks (T3, T5); one restatement, then refuted-with-finding |
| R3: "not again in the report" backfires: the notice moves to the report, or is repeated | AC-1.2 strict miss | Strict scoring counts lines across the whole stream. A position phrasing is held as the T5 fallback |
| R4: Absence lines are the model's habit (`campaign-draft-polish/evidence.md:111-112`) | AC-2.6 misses; 6 of 6 has no slack | Explicit clause (d). Strict and lenient are both reported, so the cause of a miss is visible |
| R5: The "Status not readable" line leaks into readable cases | Noise, and a strict miss in the suppression sets | Any such line in a scored set counts as a strict miss. A6/A7 are disclosed only |
| R6: Small samples (A2) | Over-claiming | Report "no miss seen in m"; the README keeps "best effort" (T10) |
| R7: Changed lines over 90 slip through again | NFR-1 fails at review | D3's diff awk with the `+` stripped is in the DoD of T3, T4 and T9 |
| R8: Scorer regex false positives or negatives | Wrong rates | Distinctive fixture name (D4); a human read of every reply decides; both are recorded |
| R9: A trim weakens a protected rule | AC-3.1 regresses | D3's never-trimmed list; T7 re-measures; re-derived after fixes |
| R10: Model or CLI drift since 2.1.292 / sonnet | Rates not comparable with A3's mix | Version and model recorded per session (T1, D4); the difference stated |
| R11: The bump reads as a breach of the AC-3.2 fence | Review finding | D6 range; the user confirms (Q2) |
| R12: `.spark/.guard/` committed, or branch drift | Fence breach or surprise merge | T1 and T9 checks; `/go-live` pre-flight |
| R13: Cost: about 50 development and about 45 QA sessions | Time and tokens | `--max-turns 12`, small fixtures; T2b capped at 4 |

**Spec assumptions inherited:**
- A2: no improvement claim.
- A3: same mix.
- A4: one rule, measured together.
- A5/A6/A7: disclosed, unscored. A7 was accepted by the user at approval.

**Open questions for the user (at plan approval; neither changes the architecture):**
- **Q1:** `campaigns/README.md` has no existing figure for the other-instance notice. Under C9's "rate figures only", should T10 (a) update only existing figures that QA re-measured (default), or (b) also add one clause giving the notice and suppression rates, registered and pooled n of m? **Answered by the user: (a), existing figures only.**
- **Q2:** Confirm D6 for this loop: `/go-live` bumps `plugin.json` `0.14.1` → `0.14.2` as the release commit, and AC-3.2's fence is read on the change set before that commit, as in `campaign-draft-polish`. **Answered by the user: confirmed.**

---

## ✅ PLAN GATE

*All boxes checked → `/increment` may start. Any box open → back to `/sprint-plan`.*

- [x] Spec status is `approved` (never plan against a draft)
- [x] Architecture decision includes rejected alternatives (a decision without alternatives is a guess)
- [x] Architecture respects the constitution's technical constraints (or a conflict is recorded): Markdown only, no executable code (helpers untracked), no template or surface change (§3, NFR-3); constitution untouched; §8 performed steps only; the bump goes to `/go-live` (D6, Q2)
- [x] Every task maps to a user story — no orphan tasks, no story without tasks
- [x] Every Must AC and every applicable NFR is covered by at least one task
- [x] Every task has a checkable definition of done
- [x] Task order respects dependencies
- [x] Test strategy covers every Must story
- [x] Line budget respected: Ist 218 / Soll ~300 (excluding HTML comments; the plan has none). Self-reported, no linter checks this
- [x] Status set to `approved` by the user

## Deviations

- **T10 is `todo` by design and waits for QA once Act closes.** It cannot run before `/demo-day` has written fresh n of m (D5, C9). The review gate expects every task done, but Act closes with T1-T9 `done` and T10 `blocked`, and `/spark` continues with `/peer-review` and `/demo-day`. T10 then runs in `/increment` fix-mode, and the next gate re-checks it. This resolves the same conflict as `campaign-draft-polish` T11.

## Deviations (Act)

1. **Step 3 trim wider than listed.** D3 (i) named only "invent no structure or tracker". To pay for the notice rule's +1 line, step 3 was also reflowed with "yourself" and "still contain" dropped (`If a path is not absolute, has $ or braces, or cannot be read`). Same rules, shorter words; the "never ask for a path" and absolute-path refusals stay.
2. **T10 waits for QA**, as designed: Act closes with T1-T9 `done`; T10 runs in `/increment` fix-mode after `/demo-day`.
3. **Candidate wording abandoned.** The plan's 5-line candidate (+2) did not fit; the shipped wording is 4 lines (+1, funded by step 3's -1). The meaning parts (a)-(f) are kept, except two narrowings: "no clash" for an unreadable Status was dropped, and D1(e)'s case of a Status the agent does not know (A6, e.g. `runing`) is no longer named, so wording C says only "Unreadable Status: only \"Status not readable\"". T6 saw the A6 session stay silent; it is unscored. The "Go on" flow is kept as "Then go on".
4. **Two development restatements, not one.** After T5's first rates (notice 7 of 10, suppression 5 of 6 on wording B) the user answered "another rewording" and wording C was run. C met the notice floor (9 of 10) and suppression (6 of 6) but not `draft`/`halted` (4 of 6). No further rewording; it goes to review and QA.
