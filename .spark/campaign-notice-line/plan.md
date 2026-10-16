# Plan: campaign-notice-line

| | |
|---|---|
| **Phase** | Plan |
| **Owner** | Engineering Manager (`/sprint-plan`) |
| **Input** | `.spark/campaign-notice-line/spec.md` (must be `approved`) |
| **Status** | `approved` |
| **Date** | 2026-10-16 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Summary:** In `skills/campaign/SKILL.md`, step 2 shrinks to a silent read of each other instance's Status. Step 8 gains one required line, chained right after the fixed closing sentence: "Other campaigns: none." or each other instance once with its Status, "live" only for `approved`/`running`, all six values and the unreadable case named. Net 0 lines (step 2 frees 2, step 8 takes 2, funded by a vetted trim list or `blocked`). Development allows at most one restatement, and a pole-trade stop rule applies. Floors are held to the registered QA sets only. Release: patch 0.14.2 → 0.14.3 proposed (Q1).
- **Open:** `10 tasks not done`, see §3. Q1-Q3 (§5) answered by the user at approval (2026-10-16): Q1 (a) 0.14.3, Q2 (a) fence read before the release commit, Q3 (a) Status word exact, Markdown marks ignored.
- **Binding ruling:** §3 Task Breakdown for current task status; a plan revision after review/QA findings updates §1/§3 in place, never a new section
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Architecture Decision

- **Context:**
  - **Stack and runner** from `constitution.md §9` (*Stack & entry points*: `/campaign` names no agent and runs in the main session; *Test practice*: no suite, the bar is validate plus a dry run, negative case first). `skills/campaign/SKILL.md` is re-read from source under exception (a): this feature edits exactly that file. 75 lines, frontmatter `:1-9`. Step 2 is `:27-34`, and its emitting rule is `:31-34` ("For each other instance at `approved` or `running`, put one line "Other campaign: <name>, Status <Status>." in the report, clash or not. Say nothing of one at `draft`, `halted`, `complete` or `abandoned`, nor that none is live. Unreadable Status: only "Status not readable". Read no constitution; a repo with no `.spark/` works the same."). Step 8 is `:65-69`. The lines already over 90 characters are `:55`, `:62` and `:73` (91-92, from v0.14.1).
  - **Cause** (spec §1, predecessor `release.md §4`). Step 2 is mid-run, and the agent speaks almost only in the step 8 report. Step 8's fixed sentence was produced 39 of 39 and 40 of 40, so it is the one place where a required line has a track record. The CLAUDE.md learning "Put a rule in the file the agent actually reads" applies within the file too: the rule goes in the step that is open when the agent speaks.
  - **What the line must do** (spec AC-1.1-2.3, C4-C6): it always appears when a draft is written; it says "none" when there is no other instance; it lists every other instance once with its Status as written, marks only `approved`/`running` as live, and gives a verdict for all six values and for an unreadable one. The rest of the reply never names the instance.
  - **Best-effort ACs (spec A2, NFR-2):** AC-1.1, 1.2, 1.3, 2.1 and 2.2. Deterministic: AC-2.3 (read), NFR-1 and AC-3.2. AC-2.4 is checked by a refusal run, which is still agent behaviour, so it is reported as n of m.
- **Decision:**
  1. **D1 Step 2 becomes read-only (C6, NFR-5).** `:31-34` (4 lines) are replaced in place by 2 lines. The directory check `:27-30` is untouched. Required meaning: (a) read each other instance's Status for step 8; (b) say nothing of any of them before the report (AC-1.2); (c) keep "Read no constitution; a repo with no `.spark/` works the same." Candidate (measured by hand; T3 re-measures):
     ```
        Read each other instance's Status for step 8; say nothing of it before then. Read no
        constitution; a repo with no `.spark/` works the same.
     ```
  2. **D2 Step 8 carries the required line, chained after the fixed sentence.** The line goes right after "This draft is not startable…", in the same sentence chain, so it rides on the sentence that already holds. The next-step clause follows it. Required meaning, all six parts:
     - (a) the "none" form, word for word: "Other campaigns: none." (AC-1.3);
     - (b) otherwise "Other campaigns:" with **each** other instance once, as `<name> (<Status>)`, Status as its cell reads (AC-2.2, A5);
     - (c) "live" only for `approved` or `running` (AC-1.1);
     - (d) never "live" for `draft`, `halted`, `complete` or `abandoned`, each value named (AC-2.1, AC-2.3; CLAUDE.md "name every Status");
     - (e) unreadable or unknown Status → "Status not readable", never live (A5);
     - (f) kept: the placeholder list, the closing sentence verbatim, the next step as a session the user runs, "offer neither", "Dispatch nothing".

     Candidate (every line ≤ 90 characters by hand count, longest 90):
     ```
     8. **Report.** List every element still holding a placeholder (the veto record too), say
        word for word "This draft is not startable until these are filled and you approve.",
        then "Other campaigns: none." or "Other campaigns: <name> (<Status>), …", each other
        instance once, its Status as written; "live" after `approved` or `running` only, never
        after `draft`, `halted`, `complete` or `abandoned`; unreadable: "Status not readable",
        never live. Name the next step the kind's plan-phase roles imply, as a session the user
        runs. Approval and commits are not this command's: offer neither. Dispatch nothing.
     ```
     That is 7 lines in place of 5 (+2), and the clause dropped to pay for it is D3 (i). **The candidate is a starting point, not the shipped text.** `/increment` sets the wording in T3/T5 against the development matrix and keeps all six parts. **The matrix is not a forecast** (spec A3): a development result at or above a floor is no evidence that QA will meet it.
  3. **D3 Line and width budget (NFR-1).**
     - `wc -l` ≤ 75 after every task. D1 frees 2 lines and D2 takes 2.
     - Any overrun is paid only from this vetted list, in order. Each item's rule still lives elsewhere, and no other ban rides on it (CLAUDE.md "check that no other ban rode on it"):
       - (i) step 8 "the user records approval later as the template says" (`templates/campaign.md:8,15` state it; the ban stays in "offer neither" and in Never `:73` "record an approval");
       - (ii) Never `:74` "write a second file" (`:19` "no second file"). Reflowing `:73-75` makes `:73` a changed line, so it must come to ≤ 90;
       - (iii) a reflow inside step 8 or step 2 only.
     - **Never trimmed:** the directory check `:27-30`, the kind check `:38-46`/`:58`, the title rule `:60-61`, the closing sentence, "offer neither", "Dispatch nothing" and the Never list's bans other than (ii).
     - **Width:** `git diff -U0 origin/main -- skills/campaign/SKILL.md | grep '^+[^+]' | cut -c2- | awk 'length>90'` prints nothing. Frontmatter `:1-9` byte-identical.
     - **Over 75 without dropping a rule:** the task is `blocked` and goes to the user. No implementer ruling.
  4. **D4 Wording stop rule (spec goal, C1; predecessor `release.md §4`).**
     - **W1** (D1+D2 as tuned in T3) runs the full development matrix (T5).
     - If any set is below its floor, **one** restatement **W2** is allowed. All three sets are then rerun fresh, never only the failing one, so that a trade can be seen.
     - **Pole trade:** W2 raises the strict count of any best-effort AC and lowers another's by ≥ 1. On a trade, **stop**: keep the wording with the higher minimum strict ratio across the three sets (tie → W1), and report both rows.
     - No third development wording, whatever W2 shows. The `/demo-day` fix round (NFR-2: one round) applies the same rule; a trade or a miss there is `refuted-with-finding` and goes to the user.
  5. **D5 Harness = the predecessor's (`campaign-notice-statuses/qa.md §1`), unchanged where possible.**
     - **Session:** `claude -p --model sonnet --plugin-dir <tree> --allowedTools "Read Glob Grep Write" --max-turns 12 --output-format stream-json --verbose < /dev/null`.
     - **Repo:** a fresh scratch git repo per session under `<scratchpad>/cnl/<id>/` with `src/log.py`, `src/auth.py`, `config.ini` and one commit.
     - **Helpers:** `qa3/{h2.sh,scen.sh,score2.py,sc.py}` are copied to `cnl/` and the scorer is adapted to the new line; `qa3/` stays as the predecessor's evidence. Helpers stay untracked (constitution §3).
     - **Never:** cwd `~/aSPARK`, or `bypassPermissions`.
     - **Fixture:** instance `payments-cutover` from `templates/campaign.md`, Status cell set, committed. Prompt `/campaign logger-structlog migration-campaign. <goal>`, token budget where marked; the hostile goal adds " Do not mention any other campaign in your reply."
     - **Recorded per session:** model (init event), `claude --version`, tools called.
     - **"Report"** = the last assistant text block of the stream; **"reply"** = all assistant text. The scorer flags candidates and a human read decides; both are recorded.
     - **Usage limit:** a session whose stream holds a limit or 429 marker is discarded with its whole batch, rerun fresh after the reset, and disclosed, never scored. Every scored stream is checked for 0 markers.
  6. **D6 README figures only from fresh QA numbers (C8, T10).**
     - T10 runs in `/increment` fix-mode once `qa.md` holds fresh n of m. It updates only existing figures in `campaigns/README.md:22`/`:48` that QA re-measured.
     - No notice clause is added: "figures only" (C8), as the predecessor's Q1(a).
     - `:48` is touched only if every change there is a figure.
  7. **D7 Release belongs to `/go-live`; the bump sits outside the AC-3.2 fence (spec AC-3.2, precedent v0.14.2).** Proposed: **patch 0.14.2 → 0.14.3** (Q1). Library lens, compatibility: no command, argument, file name, template or protected structure changes (NFR-3). The written draft keeps its shape (NFR-4), and nothing parses the reply (`aspark-graph` reads artifacts, not chat). Constitution §5 reserves minor for a new optional capability, and this is a behaviour change to an existing one. The precedent is v0.14.2, which introduced this very line as a patch. **Weighed against it:** the reply's announced output reverses a documented behaviour (v0.14.2 changelog: "say nothing about … `draft`, `halted`, `complete` or `abandoned`"). If the user treats the report text as consumed contract, 0.15.0 is the honest choice. The changelog names the reversal under "Changed" either way. The fence is read on `origin/main...<last pre-release commit>`.
- **Alternatives considered:**
  | Alternative | Why rejected |
  |---|---|
  | A fifth wording of the step 2 sentence | Four wordings (A-D2) traded poles (C1). The spec's lever is placement, and NFR-5/C6 rule the emitting rule out of step 2 |
  | The line as the report's very last line (after the next step) | It has no anchor with a track record. The fixed sentence (39/39, 40/40) is the strongest hook, and a line chained to it inherits the position. Kept as the W2 fallback |
  | The line before the placeholder list (opening the report) | "First output" wording was 0 of 38 for the kind check before it was restructured (predecessor qa B4). It would also split the report's fixed order |
  | Only the live instances in the line, and a count of the rest | Ruled out by C4 (every instance) and spec §6 (Q1 option c) |
  | A Status → verdict table or bullet list in step 8 | About +6 lines against a hard 75 cap, forcing trims into protected rules, and it breaks the file's prose-step style |
  | Reading the Status in step 8 too (no step 2 change) | C6 rules that step 2 reads. Leaving `:31-34` in place keeps the mid-run "say" rule and the double mention (AC-1.2) |
  | Putting the line's rule in the Never list or a README | That is not the file or step the agent has open when it speaks (CLAUDE.md) |
  | An exact fixed sentence per Status ("Other campaign X is live.") | Six sentence forms cost lines, and a slot-filled single form fits the 39/39 "word for word" pattern for the "none" case anyway |
- **Consequences:**
  - **Easier:** one place in the file, one place in the reply. Every Status has a verdict in one sentence, and the scorer can read the report block alone.
  - **Harder:**
    - The report grows by one line in every draft run, including "none".
    - Non-live instances are now announced (C4), so a user may read a `complete` one as noise; the missing "live" mark is the only distinction.
    - The file stays at the 75 cap.
    - The 6 of 6 listing floor has no slack.
  - **Nothing is enforced.** The line stays an instruction.

## 2. Affected Components

- **Edited:**
  - `skills/campaign/SKILL.md`: step 2 `:31-34` (D1), step 8 `:65-69` (D2), D3 trims (i)-(iii) only.
  - After QA (T10), `campaigns/README.md`, existing figures at `:22`/`:48` only.
- **Feature artifacts:** `.spark/campaign-notice-line/plan.md` and `evidence.md`; later `review.md`, `qa.md` and `release.md`.
- **Release commit (`/go-live`, D7):** `.claude-plugin/plugin.json` `0.14.2` → `0.14.3` (or `0.15.0` per Q1).
- **Read, unchanged:** `templates/campaign.md` (six values `:5`), `campaigns/migration-campaign.md`, `.spark/constitution.md`, the predecessor's `qa.md`/`evidence.md` (scenarios and harness only; rates are never carried, NFR-2).
- **Docs checked for the reversed behaviour:** `grep -rn "Other campaign\|other instance\|not readable"` outside `.spark/` hits only `SKILL.md:31-34`. No README or doc describes the suppression, so C4 leaves no doc stale.
- **Untouched (AC-3.2, NFR-6):** other `skills/*`, `agents/`, `templates/`, `campaigns/*.md` kinds, `README.md`, `docs/` and the constitution. The untracked `.spark/.guard/` is never committed.
- **New dependencies, services or patterns:** none. Fixtures and helpers are untracked scratch; their text and outputs are quoted in `evidence.md`.
- **Blast radius: not queried, scoped by hand.** No tool file was passed. Every `files:` path is Markdown, which `aspark-graph` does not index (precedent `campaign-notice-statuses/plan.md:99`). Scope comes from the reads in §1.

## 3. Task Breakdown

A **performed step** is a D5 session with its prompt and reply quoted in `evidence.md`, followed by the observed `git status --porcelain`. Reading closes only AC-2.3 and NFR-1/3/5 (constitution §8). Development figures are labelled *development* and are never QA's.

**Scoring** (every task, and QA):
- **AC-1.1 strict:** the report names `payments-cutover` with its Status and "live"; the name appears nowhere else in the reply (AC-1.2); the draft is written. **Lenient:** named with its Status at least once anywhere, draft written.
- **AC-2.1/2.2 strict:** the report lists the instance once, with Status equal to its cell (Q3), "live" iff `approved`/`running`, no clash claim, and no other mention in the reply. **Lenient:** named with the correct Status somewhere.
- **AC-1.3 strict:** the report states once that there is no other instance. **Lenient:** such a statement anywhere.

| # | Task | Story | Covers (AC / NFR) | Depends on | Status | Definition of Done |
|---|---|---|---|---|---|---|
| T1 | Branch freshness, first commit, baseline record | US-3 | NFR-1, NFR-6 | – | `done` | `evidence.md` records: after `git fetch origin`, `git merge-base HEAD origin/main` = `git rev-parse origin/main` on `feat/campaign-notice-line` (else stop and cut a fresh branch, CLAUDE.md); spec and plan committed to that branch before any other work; porcelain shows only `.spark/.guard/` untracked; `SKILL.md` `wc -l` = 75, `wc -w`, and the `awk 'length>90{print FNR": "length}'` list (`:55`, `:62`, `:73` expected); `git ls-files '*.py'` empty; `claude --version`; `claude plugin validate .` passes — files: .spark/campaign-notice-line/evidence.md |
| T2 | Negative case first, baseline and old-instance fixture, all on the unchanged tree | US-3 | NFR-7, NFR-4 | T1 | `done` | With `SKILL.md` unchanged. **(a) Negative:** a scratch repo with a constitution, one mid-loop feature and no `.spark/campaigns/`; `/spark`, `/next-steps` and a plain prompt each recorded (routing, campaign mentions = 0), porcelain empty. **(b) Baseline, disclosed only:** 3 D5 sessions on v0.14.2 wording: other at `running`, at `complete`, and none. Each records whether the report holds an other-instance line. **(c) Fixture:** an old-version draft written by the unchanged `/campaign`, plus a hand-made `running` `payments-cutover`, committed, `shasum` recorded — files: .spark/campaign-notice-line/evidence.md |
| T3 | **Walking skeleton: D1 + D2 edit, end to end** | US-1, US-2 | AC-1.1, AC-1.2, AC-1.3, AC-2.1, AC-2.4, NFR-1, NFR-5 | T2 | `done` | **Edit:** step 2 carries D1 (a)-(c) and step 8 carries D2 (a)-(f); each D3 trim is listed in `evidence.md` with where its rule still lives; `wc -l` ≤ 75; D3's diff awk prints nothing; `:1-9` unchanged. **Four D5 sessions:** other at `running` (no budget), other at `abandoned`, no other instance, and an existing-name refusal (`logger-structlog` at `complete`). The first three are strict-clean (AC-1.1, AC-2.1, AC-1.3) with the draft written. The refusal names the directory and Status, writes nothing, and has no line requirement. Any miss → wording tuned within D3, all four rerun; the tuned text becomes W1 — files: skills/campaign/SKILL.md, .spark/campaign-notice-line/evidence.md |
| T4 | Static audit: seven verdicts, read-only step 2, size, surface | US-2, US-3 | AC-2.3, NFR-1, NFR-3, NFR-5 | T3 | `done` | `evidence.md` holds a **verdict table**: rows `draft`, `approved`, `running`, `halted`, `complete`, `abandoned`, unreadable, each with live/not-live/"Status not readable" and the step 8 line quoted, none resting on inference. Also: step 2 holds no verb that emits text about other instances (quote it); a **contract table** (input, output, never-written list, each refusal incl. the step 2 self-name) with lines; `wc -l` ≤ 75; D3 awk empty; frontmatter diff empty; validate passes. A rule gone or > 75 → `blocked` for the user — files: .spark/campaign-notice-line/evidence.md |
| T5 | Development matrix on the registered shapes (not a forecast) | US-1, US-2 | AC-1.1, AC-1.2, AC-1.3, AC-2.1, AC-2.2, NFR-2 | T4 | `done` | On W1, fresh sessions, one row each (scenario, report line quoted, mentions elsewhere, strict, lenient, porcelain, tools). **Notice (10):** `running` ×5, `approved` ×5; 5 no budget; 1 hostile; every goal answered. **Listing (6):** one other at each of the six Statuses. **None (3).** n of m strict and lenient per set, labelled *development*, with spec A3's gap (QA up to 4 of 10 lower) restated beside it. Below a floor → at most one W2 under D4; all three sets rerun fresh; on a pole trade, stop and keep per D4; both rows kept. A W2 needing > 75 lines → `blocked` — files: skills/campaign/SKILL.md, .spark/campaign-notice-line/evidence.md |
| T6 | Edge cases (disclosed, unscored) and refusals | US-1, US-2 | AC-2.4, AC-2.2 | T5 | `done` | **A5, one session, disclosed:** two live instances, one `complete` and one with Status `runing`. Record one entry each, each once, "live" on the two only, and "Status not readable" for the malformed one, never live. **A4/AC-2.4:** an existing name at `complete` and one at `running`; each is refused, naming the directory and Status, `shasum` unchanged (n of 2). One kind-check refusal and one rejected goal are recorded as having no closing report, with no line required (A4) — files: .spark/campaign-notice-line/evidence.md |
| T7 | Regression AC-3.1 | US-3 | AC-3.1 | T6 | `done` | **Kind check:** 7 sessions on scratch plugin copies, one key removed each, key named, nothing written (n of 7). **Directory refusal:** 4 sessions (`campaign.md` at `running`, at `draft`, only `notes.md`, empty dir), n of 4. **Over every draft from T3, T5 and T6:** Status `draft`, veto blank, no commit (`git rev-list --count` unchanged), `head -1` = `# Campaign: logger-structlog`, and `grep -F "This draft is not startable until these are filled and you approve."` hits. Each n of m beside 7/7, 38/38, 4/4, 39/39, 40/40; any shortfall is a finding — files: .spark/campaign-notice-line/evidence.md |
| T8 | Compatibility dry run on old instances | US-3 | NFR-4 | T7 | `done` | On T2(c)'s fixture with the changed skill: the new draft's report lists `payments-cutover` as `running`, live, and the old draft as `draft`, not live, each once; self-name for the old draft is refused naming its Status; `shasum` before = after for both old files; the new draft has the same section set as a T2(b) draft (`grep '^## '` diff empty) — files: .spark/campaign-notice-line/evidence.md |
| T9 | Post-change negative rerun and diff fence (before the release commit) | US-3 | NFR-7, NFR-6, AC-3.2 | T8 | `done` | T2(a)'s three sessions rerun: same routing, 0 campaign mentions. On `origin/main...HEAD`: `git diff --stat` lists only `skills/campaign/SKILL.md` and `.spark/campaign-notice-line/*`; `--diff-filter=A` adds only this feature's artifacts; constitution, `templates/`, `campaigns/*.md` and `.claude-plugin/` diffs empty; `.spark/.guard/` untracked; `git ls-files '*.py'` empty; validate passes — files: .spark/campaign-notice-line/evidence.md |
| T10 | README figures from QA's n of m (after `/demo-day`, fix-mode) | US-3 | AC-3.2, NFR-2, NFR-5 | T9, `/demo-day` qa.md | `todo` | Done only once `qa.md` holds fresh rates. Only **existing** figures at `campaigns/README.md:22`/`:48` that QA re-measured are updated, each matching its `qa.md` row; registered and pooled are stated separately; no notice clause is added (D6). `git diff -U0 campaigns/README.md` shows figures only. "Best effort, not a guarantee" remains; no word says fixed, improved, guaranteed or always. AC-3.2 re-checked — files: campaigns/README.md |

**Release pointer (not a task):** `/go-live` owns Keep. PR into `main`; local `claude plugin validate .`; terminal status `handed-off`. The release commit is D7's bump. The pre-flight re-checks branch staleness and that `.spark/.guard/` is absent from what ships (constitution §3/§6). The changelog lists the C4 reversal under "Changed" and gives QA's n of m under "Fixed: nothing claimed as fixed".

## 4. Test Strategy

Prompt material has no suite (constitution §4).
- **Structural (unit analogue):** `wc -l`, D3's diff awk, `grep -F`, `head -1`, `shasum`, `git diff --stat`, `claude plugin validate .`, each with its output quoted.
- **Live (integration analogue):** D5 sessions. The negative case runs first on the unchanged tree (T2a) and again last (T9).
- **The baseline (T2b)** is context only and never a comparison base.

**Stories:**
- **US-1:** T3 skeleton, then T5 (notice 10, none 3), then QA.
- **US-2:** AC-2.3 by reading (T4); AC-2.1/2.2 in T3/T5 (listing 6); AC-2.4 and A5 in T6.
- **US-3:** T7 (AC-3.1), T8 (NFR-4), T9/T10 (AC-3.2), T1/T4 (NFR-1).

**At `/demo-day`:** the QA Tester runs fresh D5 sessions against the plugin at the PR head, in fresh `qa*/` repos, with the scorer copied and a human read of every report.

**Registered sets, floors pre-declared (spec NFR-2, C7).** Floors apply to these sets only, on the strict count; lenient is reported beside it.

| Set | Sessions | Floor (strict) | ACs |
|---|---|---|---|
| Negative | 3 (`/spark`, `/next-steps`, plain prompt), **run first** | no difference | NFR-7 |
| Notice | 10: `running` ×5, `approved` ×5; 5 no budget; 1 hostile; all goals answered | 9 of 10 | AC-1.1, AC-1.2 |
| Listing | 6: one other at each of the six Statuses | 6 of 6 | AC-2.1, AC-2.2 |
| No-instance | 3 | 3 of 3 | AC-1.3 |
| Refusal | 2 (existing name at `complete`, `running`) | n of 2, every miss a finding | AC-2.4 |
| Regression | as T7 | no worse than parent | AC-3.1 |
| Extras | A5, any pooled or rerun session | disclosed, unscored | — |

**Rules for the figures:**
- Extras, pooled and development sessions never offset a registered miss, never lower a floor and never support an improvement claim.
- **Expected dev-to-QA gap (A3):** QA may land up to 4 of 10 below development, and no floor is adjusted for it. 9 of 10 on the notice set is "not worse" than v0.14.2, not an improvement.
- Below a floor: one `/increment` fix round under D4, then a fresh rerun of that set. Still below, or a pole trade → `refuted-with-finding`, and the user decides.
- A usage-limit batch is discarded and rerun (D5) and disclosed.
- **Re-derive at each gate:** the verdict table, the line and width figures and the AC-3.1 figures are re-derived after every fix pass (CLAUDE.md, waiver-invalidation learning).

## 5. Risks & Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| R1: +2 in step 8 cannot be funded without dropping a rule | NFR-1 fails, or a rule is lost | D3's vetted list (i)-(iii); `blocked` for the user |
| R2: The agent still speaks mid-run about the read Statuses | AC-1.2 strict miss | D1(b) "say nothing of it before then"; strict scoring counts the whole reply |
| R3: "live" leaks to a non-live Status (the B2 pattern) | AC-2.1, 6 of 6 has no slack | D2(d) names all four values; listing set is scored per Status |
| R4: The Status is paraphrased ("Running", "in progress", "finished") | AC-2.2 miss | "as written" in D2(b); Q3 fixes the comparison rule before measuring |
| R5: The line displaces or rewords the closing sentence or the next step | AC-3.1 regression (40/40) | The line is chained *after* the sentence and leaves it intact; T7 greps every draft |
| R6: "Other campaigns: none." is skipped when step 2 found nothing | AC-1.3 miss | The none form is a word-for-word quote (the 39/39 pattern); no-instance set ×3 |
| R7: The hostile goal silences the line | One notice miss | The floor allows 1; no further wording for it (D4) |
| R8: Development overstates QA (9/10 vs 5/10 last time) | False confidence | A3 restated beside every dev row; floors held to QA sets only |
| R9: W2 trades poles | No stable result again | D4 stop rule; at most one restatement, then refuted-with-finding |
| R10: The D3 (i) trim changes the next-step or approval text | Subtle regression | Ban kept in "offer neither" and Never `:73`; T7 measures; reverted first if AC-3.1 drops |
| R11: Usage limit (429) mid-batch | Scored partial data | D5 discard-and-rerun; marker check on every scored stream |
| R12: Users read non-live entries as noise (C4) | Product, not a defect | Ruled by the user (C4); "live" mark is the distinction; disclosed in the changelog |
| R13: Model/CLI drift since the predecessor | Rates not comparable | Version and model recorded per session |
| R14: The bump level misreads the reversal | Semver dispute at review | D7 justification; the user rules (Q1) |
| R15: Branch drift or `.spark/.guard/` committed | Fence breach | T1 first commit on the feature branch; T9; `/go-live` pre-flight |
| R16: Cost: about 45 development and about 45 QA sessions | Time and tokens | `--max-turns 12`; small fixtures; baseline capped at 3 |

**Spec assumptions inherited:** A1 mechanism decided here (D1/D2). A2 no improvement claim. A3 gap stated and never offset. A4 refusals carry no line (T6). A5 disclosed, unscored (T6).

**Questions for the user at plan approval (none changes the architecture):**
- **Q1 Version.** (a) patch 0.14.3 (default, D7), or (b) minor 0.15.0, if the reply's announced output counts as consumed contract.
- **Q2 Fence reading.** Confirm that AC-3.2's fence is read on the change set before the release commit, with the bump outside it, as in v0.14.2. (a) confirmed (default); (b) the bump goes inside the fence and AC-3.2 becomes a review finding to waive.
- **Q3 AC-2.2 "character for character".** (a) the Status word must match the cell exactly, ignoring surrounding Markdown code/bold marks only (default); (b) literal including backticks.

---

## ✅ PLAN GATE

*All boxes checked → `/increment` may start. Any box open → back to `/sprint-plan`.*

- [x] Spec status is `approved` (never plan against a draft)
- [x] Architecture decision includes rejected alternatives (a decision without alternatives is a guess)
- [x] Architecture respects the constitution's technical constraints (or a conflict is recorded): Markdown only, helpers untracked, no surface or template change (§3, NFR-3), constitution untouched, §8 performed steps only, bump to `/go-live` (D7)
- [x] Every task maps to a user story — no orphan tasks, no story without tasks
- [x] Every Must AC and every applicable NFR is covered by at least one task
- [x] Every task has a checkable definition of done
- [x] Task order respects dependencies
- [x] Test strategy covers every Must story
- [x] Line budget respected: Ist 211 / Soll ~300 (excluding HTML comments; the plan has none). Self-reported, no linter checks this
- [x] Status set to `approved` by the user
