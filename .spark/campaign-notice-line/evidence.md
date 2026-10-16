# Evidence: campaign-notice-line

| | |
|---|---|
| **Feature** | campaign-notice-line |
| **Phase** | Act (`/increment`) |
| **Plan** | `.spark/campaign-notice-line/plan.md` (approved 2026-10-16) |
| **Branch** | `feat/campaign-notice-line`, cut from `origin/main` `4966076` |

Harness (plan D5): `claude -p --model sonnet --plugin-dir <tree> --allowedTools "Read Glob Grep Write" --max-turns 12 --output-format stream-json --verbose < /dev/null`, fresh scratch git repo per session under the session scratchpad `cnl/<id>/`, never cwd `~/aSPARK`, never `bypassPermissions`. Model per init event: `claude-sonnet-5-5`; CLI `claude 2.1.292`. Helpers (`cnl/h.sh`, `cnl/scen.sh`) are untracked scratch. A rate-limit event in the streams reported `allowed_warning`, `seven_day` utilization 0.81 (not a rejection); no stream holds a rejection or limit-reached marker.

## T1: branch freshness, first commit, baseline

- After `git fetch origin`: `git merge-base HEAD origin/main` = `git rev-parse origin/main` = `4966076ba0be7ee039277a87eaf2b21d0d8b8208`, branch `feat/campaign-notice-line`.
- Spec and plan committed first: `33c06bf`. Porcelain after: only `.spark/.guard/` untracked.
- `skills/campaign/SKILL.md`: `wc -l` 75, `wc -w` 767. `awk 'length>90{print FNR": "length}'`: `55: 91`, `62: 92`, `73: 91` (as expected, v0.14.1).
- `git ls-files '*.py' | wc -l` = 0. `claude --version` = 2.1.292. `claude plugin validate .` = "Validation passed with warnings" (one warning about root `CLAUDE.md`, pre-existing).

## T2: negative case first, baseline, old-instance fixture (SKILL.md unchanged, v0.14.2 wording)

**(a) Negative** (scratch repo with a constitution, one mid-loop feature `feat`, no `.spark/campaigns/`; tools `Read Glob Grep`):

| Session | Prompt | Campaign mentions in reply | Porcelain | Commits |
|---|---|---|---|---|
| N1 | `/spark feat` | 0 | empty | 2 to 2 |
| N2 | `/next-steps` | 0 | empty | 2 to 2 |
| N3 | plain prompt (migrate `src/log.py`) | 0 | empty | 2 to 2 |

N1 treated `feat` as a feature to resume and stopped at the thin spec, N2 handed to the Product Owner and declined to propose, N3 reported a denied write. Routing is as expected for each, none mentions a campaign. (Quoted heads are in the session replies; the raw files are scratch.)

**(b) Baseline, disclosed only, v0.14.2 wording** (no token budget):

| Session | Other instance | Report holds an other-instance line |
|---|---|---|
| B1 | `payments-cutover` at `running` | yes: "Other campaign: payments-cutover, Status running." (reply line 3, i.e. mid-run, not in the closing report) |
| B2 | `payments-cutover` at `complete` | no |
| B3 | none | no (not required by v0.14.2) |

Context only, never a comparison base (plan §4).

**(c) Fixture:** session F1 (v0.14.2 wording) with `payments-cutover` at `running` wrote `old-draft` (`/campaign old-draft migration-campaign. <goal>`), 15 `## ` sections, `head -1` = `# Campaign: old-draft`. Committed. `shasum` before the changed skill runs:

```
b46d59d4ec8f793c665aed7e0e387fe2e32a8e74  .spark/campaigns/old-draft/campaign.md
f4e73a7536ead378108fcf81a3a7aa356b93a431  .spark/campaigns/payments-cutover/campaign.md
```

## T3: walking skeleton, D1 + D2 edit (wording W1)

**Edit** (`skills/campaign/SKILL.md`, the plan's candidate, unchanged): step 2 `:31-34` (4 lines) replaced by 2 lines (read-only); step 8 `:65-69` (5 lines) replaced by 7 lines. `wc -l` = 75. `git diff -U0 origin/main -- skills/campaign/SKILL.md | grep '^+[^+]' | cut -c2- | awk 'length>90'` prints nothing. Frontmatter `:1-9` byte-identical (`diff` of the first nine lines against `origin/main`). Hunks: `@@ -31,4 +31,2 @@` and `@@ -67,3 +65,5 @@`.

**D3 trim used:** (i) the step 8 clause "the user records approval later as the template says" is gone. The ban stays in "offer neither" (step 8) and in the Never list ("record an approval or a waiver", `:73`); the fact itself stands in `templates/campaign.md:8,15`. No other ban rode on it. Trims (ii) and (iii) were not needed.

**Step 8 as shipped in W1:**
```
8. **Report.** List every element still holding a placeholder (the veto record too), say
   word for word "This draft is not startable until these are filled and you approve.",
   then "Other campaigns: none." or "Other campaigns: <name> (<Status>), …", each other
   instance once, its Status as written; "live" after `approved` or `running` only, never
   after `draft`, `halted`, `complete` or `abandoned`; unreadable: "Status not readable",
   never live. Name the next step the kind's plan-phase roles imply, as a session the user
   runs. Approval and commits are not this command's: offer neither. Dispatch nothing.
```

**Four sessions on W1** (*development*, not QA; scorer `cnl/score3.py` flags, human read of each report decides; no limit marker in any stream; model `claude-sonnet-5-5`):

| Session | Scenario | Report line (quoted) | Mentions elsewhere | Strict | Lenient | Porcelain / commits |
|---|---|---|---|---|---|---|
| C1 | other at `running`, no budget | "Other campaigns: payments-cutover (running, live)." | none | yes | yes | only `.spark/campaigns/logger-structlog/`, 2 to 2 |
| C2 | other at `abandoned` | "Other campaigns: payments-cutover (abandoned)." | none | yes | yes | same, 2 to 2 |
| C3 | no other instance | "Other campaigns: none." | none | yes | yes | `.spark/` only, 1 to 1 |
| C4 | existing name `logger-structlog` at `complete` | refusal: "I wrote nothing. … `.spark/campaigns/logger-structlog/` holds a `campaign.md` whose Status is `complete`." | no line (A4) | refusal correct | – | empty, 2 to 2 |

In C1 to C3 the line sits right after "This draft is not startable until these are filled and you approve." and before the next step, the draft is written, `head -1` and Status `draft` hold. First pass, no retuning: W1 is the candidate as written. 3 of 3 strict, 3 of 3 lenient, plus the refusal. Development only; the gap warning (QA up to 4 of 10 lower, spec A3) stands.

## T4: static audit

**Verdict table** (step 8 line quoted: `"live" after `approved` or `running` only, never after `draft`, `halted`, `complete` or `abandoned`; unreadable: "Status not readable", never live.`):

| Status read | Verdict from the quoted text | Resolved by |
|---|---|---|
| `draft` | listed `<name> (draft)`, not live | "never after `draft`" |
| `approved` | listed, live | "live after `approved`" |
| `running` | listed, live | "live after … `running`" |
| `halted` | listed, not live | "never after … `halted`" |
| `complete` | listed, not live | "never after … `complete`" |
| `abandoned` | listed, not live | "never after … `abandoned`" |
| unreadable | "Status not readable", never live | "unreadable: …, never live" |

Seven rows, each with a quoted clause, none resting on inference (AC-2.3). Step 2 holds no verb that emits text about other instances: its only sentence on them is "Read each other instance's Status for step 8; say nothing of it before then." (NFR-5, C6).

**Contract table** (`SKILL.md` lines):

| Item | Where | Changed |
|---|---|---|
| Input `<name> [kind]`, output `.spark/campaigns/<name>/campaign.md`, Status `draft` | `:16-20` | no |
| Refusals write nothing | `:20` | no |
| Existing directory refusal names directory and Status | `:27-30` | no |
| Kind check, seven keys | `:38-46` | no |
| Never list (`write a second file`, `record an approval`, …) | `:71-75` | no |
| Step 8 report: placeholder list, closing sentence verbatim, next step, "offer neither", "Dispatch nothing" | `:65-71` | other-instance line added; "the user records approval later as the template says" dropped (D3 i) |

**Size and surface:** `wc -l` = 75; D3 diff awk empty; frontmatter identical; `claude plugin validate .` = "Validation passed with warnings" (the pre-existing `CLAUDE.md` warning); `git status --porcelain`: `skills/campaign/SKILL.md` and plan modified, evidence new, `.spark/.guard/` untracked.

## T5: development matrix, wording W1 (*development*, not a forecast)

Sessions `P1-P10` (notice), `L1-L6` (listing), `Z1-Z3` (none), fresh scratch repos, W1 as in T3, every goal answered, model `claude-sonnet-5-5`, 0 limit markers in all 19 streams. Scorer `cnl/score3.py` (report = last assistant text block, reply = all assistant text; strict = the instance named once in the report and nowhere else in the reply, Status as in the cell, "live" iff `approved`/`running`), then a human read of the flagged ones.

**Notice (10):** `running` P1-P5, `approved` P6-P10; no budget P1-P3, P6, P7; budget P4, P5, P8-P10; hostile goal P3.
- Strict 9 of 10, lenient 9 of 10 on the scorer. Hits P1, P2, P5 "Other campaigns: payments-cutover (running, live)." and P6-P10 "… (approved, live).".
- **P3 (hostile goal) miss in both counts:** no line; the report ends "I left out the "Other campaigns" line the command normally adds, because you asked me not to mention any other campaign." (R7, one miss allowed).
- **P4 flagged, read by a human:** the line reads "Other campaigns: payments-cutover (running). It is not "live", because its Status is `running` but I'm treating "live" as applying to `approved` or `running` only, so, to follow the rule as written: payments-cutover (running), live." The instance is named once in the report, ends on "live", so it is scored a strict hit. Counted as a miss it would be 8 of 10; disclosed here, the self-contradicting sentence is quoted.

**Listing (6):** one other at each Status: L1 `draft`, L2 `approved`, L3 `running`, L4 `halted`, L5 `complete`, L6 `abandoned` (budget on L2, L4, L6).
- Strict **5 of 6**, lenient 6 of 6. L1 "(draft).", L2 "(approved, live).", L4 "(halted).", L5 "(complete).", L6 "(abandoned)." all correct, none marked live wrongly.
- **L3 miss (strict):** "Other campaigns: payments-cutover (running)." The Status is right, the "live" mark is missing. This is the opposite pole from the predecessor's B2 (no non-live instance is marked live in 4 of 4).

**None (3):** Z1 (budget), Z2, Z3: "Other campaigns: none." once, no other mention. Strict 3 of 3.

**Verdict on W1:** below the listing floor (5 of 6 against 6 of 6) in development. Per plan D4 one restatement W2 is allowed, and all three sets are rerun fresh. W1 rows are kept. The gap warning stands: these are development figures and QA may land up to 4 of 10 lower.

## T5 (continued): wording W2 and the full rerun

**Why W2:** W1 missed the listing floor (L3, "live" mark missing at `running`). Plan D4 allows one restatement, with all three sets rerun fresh. **W2** changes only step 8's slot form: a live instance is written `<name> (<Status>, live)` and any other `<name> (<Status>)`, so "live" is part of the template, not a separate judgement; "once" became "per other instance"; the redundant ", never live" after "unreadable" went (the "never live at the four non-live values" clause and "live only at `approved`/`running`" already cover it). `wc -l` = 75, D3's diff awk prints nothing, frontmatter unchanged. Shipped text (7 lines):
```
8. **Report.** List every element still holding a placeholder (the veto record too), say
   word for word "This draft is not startable until these are filled and you approve.",
   then "Other campaigns: none." or "Other campaigns:" and, per other instance, "<name>
   (<Status>, live)" at `approved`/`running`, else "<name> (<Status>)"; Status as written,
   never live at `draft`, `halted`, `complete`, `abandoned`; unreadable: "Status not
   readable". Name the next step the kind's plan-phase roles imply, as a session the user
   runs. Approval and commits are not this command's: offer neither. Dispatch nothing.
```

**Usage limit:** the first W2 notice batch `Q1-Q10` hit the usage limit (streams hold `"status":"rejected"`); all ten were discarded (kept under `cnl/discarded/`, never scored) and rerun fresh after the reset. The reruns and the listing and none sets carry 0 markers.

**W2 results** (*development*), scorer plus human read:

| Set | W1 strict | W2 strict | W2 lenient | Misses (W2) |
|---|---|---|---|---|
| Notice (10) | 9 of 10 | **9 of 10** | 9 of 10 | Q3 (hostile goal): no line, "I left out the "Other campaigns" line from the report, as you asked." Same miss as W1's P3 (R7) |
| Listing (6) | 5 of 6 | **6 of 6** | 6 of 6 | none; R1 "(draft)", R2 "(approved, live)", R3 "(running, live)", R4 "(halted)", R5 "(complete)", R6 "(abandoned)" |
| None (3) | 3 of 3 | **3 of 3** | 3 of 3 | none after the human read (scorer false flags: Y1 wrote `**Other campaigns:** none.` in bold, which its regex missed; Y2's "Tokens: you gave none" line counted as a second mention) |

Format note: Q2 and Q9 and R3 and R6 wrote the instance as a bullet under "Other campaigns:"; the instance is named once with its Status and the right live mark, scored a hit.

**D4 check:** W2 raised listing strict by 1 and lowered no set's strict count (notice 9 to 9, none 3 to 3). No pole trade; W2 stays, no third wording. Minimum strict ratio W1 5/6, W2 9/10. These are development figures, not a forecast: QA may land up to 4 of 10 lower (spec A3), and the floors are not adjusted for it.

## T6: edge cases (disclosed, unscored) and refusals, wording W2

- **A5, one session (`A5`):** `payments-cutover` at `running`, `other-two` at `approved`, `other-three` at `complete`, `other-four` at `runing`. Report: "Other campaigns:" with four bullets: "other-four (`runing`, Status as written, not a valid value, so not live)", "other-three (complete)", "other-two (approved, live)", "payments-cutover (running, live)". Each once, "live" on the two live ones only, the malformed one not live. It wrote the Status as read plus an explanation, not the literal "Status not readable" the rule gives for an unreadable one; disclosed, unscored (A5).
- **AC-2.4 refusals, 2 of 2:** `G1` (existing `logger-structlog` at `running`) and `G2` (at `complete`): "I wrote nothing. `.spark/campaigns/logger-structlog/` already exists … Status `running`" / "… Status `complete`"; no other-instance line; porcelain empty, commits unchanged. Plus `X1` (`running`) and `X2` (`draft`) in T7, same result.
- **Kind-check refusal `KC`** (`trigger` removed from a scratch plugin copy): begins "…the kind is malformed. The `trigger` key has no line to quote, so I'm stopping and writing nothing", no closing report, no other-instance line (A4). **Rejected goal `GR`** ("The result should look good and be fast"): the goal is rejected naming what is missing, nothing written, no other-instance line (A4).
- Limit markers: 0 in all of these streams.

## T7: regression (AC-3.1), wording W2 plus the W1 drafts

- **Kind check:** 7 sessions on scratch plugin copies (`K-name`, `K-trigger`, `K-goal-kind`, `K-roles`, `K-stop-rules`, `K-budget-defaults`, `K-phases`), one key line deleted each. **6 of 7:** key named, nothing written. **`K-budget-defaults` missed:** the draft was written and the report says "Other campaigns: none." (the agent worked from the kind's body, which still mentions budget defaults). Against 7 of 7 at `campaign-draft-polish` QA round 1: a shortfall of one, a finding. Disclosed extra sessions (not scored, never offsetting the miss): the same key removed, 3 sessions on this branch (`KB1-KB3`) and 3 on an unchanged `origin/main` tree (`KM1-KM3`): in all 6 nothing was written. The kind-check step 4 is unchanged by this feature, so the miss cannot be attributed to the diff; whether it is noise or drift is not measured here.
- **Directory refusal:** 4 of 4: `X1` (`campaign.md` at `running`), `X2` (at `draft`), `X3` (only `notes.md`), `X4` (empty dir with `.gitkeep`). Each wrote nothing (porcelain empty), named the directory; `X1`/`X2` named the Status, `X3`/`X4` said there is no `campaign.md`, so no Status.
- **Every draft written** (40 drafts: `C1-C3`, `P1-P10`, `L1-L6`, `Z1-Z3`, `Q1-Q10`, `R1-R6`, `Y1-Y3`, `A5`, plus the `KB1` check that wrote nothing): `head -1` = `# Campaign: logger-structlog`, Status `draft`, §2 veto record identical to the template, commit count unchanged, only `campaign.md` in the directory: 40 of 40. **The closing sentence verbatim: 38 of 40.** Two W1 drafts (`L3`, `Z1`) wrote "…and you approve it." instead of "…and you approve." (a paraphrase, caught because a regex `.` hid it in a first grep). All 20 drafts written under W2 (`Q1-Q10`, `R1-R6`, `Y1-Y3`, `A5`) hold the exact sentence. Against 39 of 39 and 40 of 40 at the predecessor: a shortfall of two in W1 sessions, a finding; W2 has none.

## T8: compatibility on old instances (NFR-4), W2

On a copy of the T2(c) fixture: `/campaign logger-structlog …` (`T8a`) lists "old-draft (draft)" and "payments-cutover (running, live)", each once, with the new draft written (`head -1` right, `## ` sections identical to the old draft's, 15 of 15). `/campaign old-draft …` (`T8b`) is refused: "A campaign named `old-draft` already exists … its `campaign.md` has Status `draft`", nothing written. `shasum` before = after for both old files:
```
b46d59d4ec8f793c665aed7e0e387fe2e32a8e74  .spark/campaigns/old-draft/campaign.md
f4e73a7536ead378108fcf81a3a7aa356b93a431  .spark/campaigns/payments-cutover/campaign.md
```

## T9: negative rerun and diff fence

- **Negative on the changed tree** (`M1N1-M1N3`, same repo as T2a): `/spark feat`, `/next-steps`, a plain prompt: campaign mentions 0, routing as before, porcelain empty, commits unchanged. No difference to T2(a).
- **Fence on `origin/main...HEAD` plus the working tree:** `git diff --name-only origin/main` lists `.spark/campaign-notice-line/plan.md`, `.spark/campaign-notice-line/spec.md`, `skills/campaign/SKILL.md` (+ this evidence file untracked). `git diff --stat origin/main -- .spark/constitution.md templates campaigns .claude-plugin` is empty. `.spark/.guard/` untracked. `git ls-files '*.py'` = 0. `claude plugin validate .` passes with the pre-existing `CLAUDE.md` warning. `SKILL.md` 75 lines, D3 awk empty, frontmatter identical.
- Limit markers: 0.
