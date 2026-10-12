# Spec: campaign-notice-statuses

| | |
|---|---|
| **Phase** | Specify |
| **Owner** | Product Owner (`/story-time`), Designer (`/look-and-feel`) |
| **Status** | `approved` |
| **Date** | 2026-10-12 |
| **Ticket** | none |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Summary:** `/campaign` step 2 still omits the other-instance notice in about 3 of 20 pooled sessions (B1) and still announces `complete`/`abandoned` instances (B2); tighten the one rule, measure fresh, say what the numbers can and cannot show.
- **Open:** `0 open` — A7 accepted as default risk by the user (approval); Q1-Q4 resolved by the user, Q5 default kept (§7 C5-C9)
- **Binding ruling:** §4 User Stories for the current stories; §7 Clarifications for what changed since the last round and why
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Problem & Goal

- **Problem:** `.spark/campaign-draft-polish/qa.md` round 1 left two Minors open. B1: the notice about another `approved`/`running` instance (parent AC-1.1) was omitted in 3 of 20 pooled sessions (A7, A12 silent; A13 said it twice); 9 of 10 on the registered ten, 17 of 20 pooled, not distinguishable from the 6-of-7 baseline. B2: step 2 names only `draft` and `halted` as never mentioned, so an instance at `complete` or `abandoned` was announced in 4 of 4 sessions (B7-B10), one session saying "I mentioned no other campaign instances" while naming it. The user who runs `/campaign <name> [kind]` either misses a live clash or is told about a finished campaign as if it mattered. The real Status set is `draft`, `approved`, `running`, `halted`, `complete`, `abandoned` (`templates/campaign.md:5`). System facts (`/campaign` run by the main session, Markdown-only plugin) from `constitution.md §9`; the line-count and wording facts re-derived from source because this feature edits exactly that file (`SKILL.md` 75 lines, step 2 at lines 31-32).
- **Goal:** step 2 states, for each of the six Status values, whether the instance is announced, and the observed rate of the notice does not fall below the floor (NFR-2) while the suppression leak for `complete`/`abandoned` is measured at zero misses in its sample.
- **Success signal:** fresh QA sessions against the installed plugin, reported as n of m per AC (NFR-2). No claim of "fixed" or "improved", only of the observed rate.
- **Why now:** low urgency, said plainly. If never built: nothing breaks; the notice is advisory (one-active-campaign is not enforced, parent A6) and a mention of a finished campaign is noise. The live reasons: B2 is a defect by omission in a rule that should name every Status, and B1 is the one story the parent spec made a Must that sits at the line.

## 2. Target Users

The maintainer or Claude Code user running `/campaign <name> [kind]` while other instances exist under `.spark/campaigns/` (parent spec's user). Read-only consumer of the reply; no new role.

## 3. Assumptions & Open Questions

| # | Assumption / Question | Resolution |
|---|---|---|
| A1 | Original phrasing: "fix the two open Minor gaps". Underlying need: the user hears about live campaigns every time, and only about those. | Accepted |
| A2 | The B1 sample cannot show an improvement: 9 of 10 versus a 17-of-20 baseline is within noise. This feature aims at "does not regress and the rule is complete", not "proves better". | Accepted risk, NFR-2 |
| A3 | B1's misses cluster in `running`, no-token-budget and goal-answered scenarios (qa.md AC-1.1 row: running 8 of 10, no budget 6 of 8). Fresh runs must reuse the same mix or the rates are not comparable. | Folded into NFR-2 |
| A4 | A fix to B2 (qa.md B2 proposes "never mention one at any other Status") may itself shift the B1 rate; the two are one rule in one place, so one feature, measured together. | Accepted |
| A5 | Several other instances at once: the rule applies per instance (one notice line each, Status named, each once); only those at `approved`/`running` are announced. Not measured by the floor sets (they use one other instance); one disclosed, unscored session (2 live + 1 `complete`) checks it reads sanely. | Accepted default |
| A6 | An other instance whose Status is a value outside the six (typo, e.g. `runing`): not announced as a live instance and not silently treated as live; the reply may say its Status is unrecognised once, naming no clash. | Accepted default, unscored |
| A7 | An other instance with no `campaign.md`, an empty one, or an unreadable Status cell (e.g. placeholder `draft \| approved \| ...` left unfilled): treated like A6, not announced as `approved`/`running`. Proposed: one neutral "Status not readable" line is allowed, counted outside the floors. Impact is low (the parent never defined it; the old rule also ignored it) but it is a new sentence in a file at the cap. | **Open**, default stands unless the user objects |
| Q1 | Line cap. | Resolved by the user: `SKILL.md` ≤ 75 lines and every changed line ≤ 90 characters (C5) |
| Q2 | Floor and session sets. | Resolved by the user: as proposed (C6) |
| Q3 | Neutral absence line. | Resolved by the user (C7) |
| Q4 | Double notice. | Resolved by the user (C8) |
| Q5 | README scope. | Not asked; default kept (C9) |

## 4. User Stories

### US-1 (Must): The other-instance notice is given every time

> As a user starting a new campaign draft while another instance is `approved` or `running`, I want to be told about it every time, with its name and Status, so that I notice a possible clash before I approve anything.

**Acceptance criteria:**

- [ ] AC-1.1: Given `.spark/campaigns/` holds another instance at `approved` or `running`, when `/campaign <name> <kind> <goal>` writes a draft, then the reply names that instance and its Status exactly once and the draft is still written. *Best-effort (NFR-2).*
- [ ] AC-1.2: Given the same fixture, when the reply is read, then the notice appears once in total, not once at the start and once at the end. *Best-effort (NFR-2); a double notice counts as a miss for AC-1.1 under the strict count, the lenient count is disclosed beside it (C8).*

### US-2 (Must): Finished and parked instances are never announced

> As a user starting a new draft, I want not to be told about instances that are not live, so that a notice always means a possible clash.

**Acceptance criteria:**

- [ ] AC-2.1: Given the only other instance is at `complete`, when a draft is written, then the reply does not name that instance or its Status. *Best-effort (NFR-2).*
- [ ] AC-2.2: Given the same with `abandoned`, then likewise. *Best-effort (NFR-2).*
- [ ] AC-2.3: Given the other instance is at `draft` or `halted`, then likewise (no regression of the F1 result, `draft` 3 of 3, `halted` 3 of 3 strict-clean at 5 of 6). *Best-effort (NFR-2).*
- [ ] AC-2.4: Given `SKILL.md` step 2, when it is read, then it gives an announce-or-silent verdict for every one of the six Status values (`draft`, `approved`, `running`, `halted`, `complete`, `abandoned`), none left to inference from the other lists. *Checkable by reading, deterministic.*
- [ ] AC-2.5: Given the user types a name that already exists, then the refusal still names that directory and its Status (parent AC-3.1): the suppression applies to *other* instances only.
- [ ] AC-2.6: Given at least one other instance exists but none is `approved` or `running`, when the reply is read, then it contains no statement about the absence of live campaigns (e.g. "no other campaign is at `approved` or `running`"); given no other instance exists at all, such a statement is allowed but not required. *Best-effort (NFR-2); a forbidden absence line counts as a miss for AC-2.1 to 2.3 under the strict count (C7).*

### US-3 (Must): No regression

> As a maintainer, I want the rest of `/campaign` to behave as before, so that tightening one rule breaks nothing.

**Acceptance criteria:**

- [ ] AC-3.1: Given the change, when the parent's kind check (7 of 7 key removals named, nothing written), Status `draft` / veto blank / no commit, existing-directory refusal, title `# Campaign: <name>` (`head -1`) and the closing sentence "not startable until these are filled and you approve" are re-run, then each shows no worse than `campaign-draft-polish` QA round 1 (7 of 7, 38 of 38, 4 of 4, 39 of 39, 40 of 40).
- [ ] AC-3.2: Given the change set, when `git diff --stat` is read, then only `skills/campaign/SKILL.md`, the best-effort rate figures in `campaigns/README.md` (C9) and this feature's `.spark/campaign-notice-statuses/` files changed; `.spark/constitution.md`, `templates/`, `campaigns/*.md` kinds and `.claude-plugin/` are untouched.

## 5. Non-Functional Requirements

| # | Category | Requirement (measurable) | How it's verified |
|---|---|---|---|
| NFR-1 | Attention budget | `wc -l skills/campaign/SKILL.md` ≤ 75, and no changed line over 90 characters (`awk 'length>90'` over the diff's added lines prints nothing); a higher count or longer line needs the user's recorded ruling, not the implementer's (C5). | `wc -l`, `awk`, /peer-review |
| NFR-2 | Best-effort rates (prompt material: likely, never certain) | **Which ACs only prompt material can make likely:** AC-1.1, 1.2, 2.1, 2.2, 2.3, 2.6. AC-2.4 and AC-2.5 are checkable by reading or a refusal run. **Pre-declared floors (C6):** the *registered set* is 10 fresh notice sessions (5 `running`, 5 `approved`; 5 with no token budget; 1 hostile goal; every goal answered), floor 9 of 10 for AC-1.1/1.2; the suppression set is 6 fresh sessions (3 `complete`, 3 `abandoned`), floor 6 of 6 for AC-2.1/2.2/2.6; `draft`/`halted` (AC-2.3) 6 of 6 on 3 + 3 fresh sessions. The floor applies to these registered sets only. Extra or pooled sessions are disclosed beside them, never used to claim improvement, to offset a registered miss or to lower the floor. A 9 of 10 at the floor is "not worse than 9 of 10 last time", not an improvement (A2). Below the floor: one fix round, then `refuted-with-finding` and the user decides. Rates are fresh, never carried from `campaign-draft-polish`; reports give n of m, the strict count and the lenient count beside it; docs never call these guarantees. | Hands-on QA per constitution §8 (performed steps only); /peer-review |
| NFR-3 | Library: public surface | No command, argument, output file name, template or protected structure (constitution §3) renamed, added or removed; `/campaign <name> [kind]` unchanged. | /peer-review |
| NFR-4 | Library: compatibility | Instances written by the previous version stay valid and untouched (`shasum` before = after); only the notice rule changes, so the draft written has the same shape. | Recorded dry run + /peer-review |
| NFR-5 | Library: contract clarity | The rule sits in step 2 (the step the agent executes when it must act; CLAUDE.md "Put a rule in the file the agent actually reads"), names every Status, and `SKILL.md` still states input, output, what is never written and each refusal. Updated README rates keep "best effort, not a guarantee" and state registered and pooled n of m separately. | /peer-review |
| NFR-6 | Stack / approval | Zero new tracked files beyond this spec set; no executable code; `claude plugin validate .` passes; constitution unchanged; nothing committed or approved by the command. Diff fence: AC-3.2. | /peer-review |
| NFR-7 | Negative case first | In a repo that never runs `/campaign` (`/spark`, `/next-steps`, a plain prompt), no ceremony output differs; run first. | Recorded dry run |

Active lens `library`: public surface, compatibility, contract clarity are NFR-3, NFR-4, NFR-5; packaging is N/A (no bundle, no dependencies). Other lenses off (constitution §2).

## 6. Out of Scope

- Constitution F8 and any constitution edit (a `/charter` matter).
- Running, resuming or routing campaigns; new kinds; a second command.
- Other `campaign-draft-polish` Minors: B3 (dropped kind heading), B4 (kind check not emitted first, 0 of 38), B5 (tool set); any new check.
- Enforcing one-active-campaign (parent A6) or blocking on a clash; the notice stays advisory.
- Making the notice deterministic: impossible in prompt material; improvement is measured, never promised.
- Any edit to `campaigns/README.md` beyond rate figures (C9), including the stale `:48` sentence unless its only needed change is a figure; any other doc.
- Scored floors for multiple other instances (A5) or malformed/Status-less ones (A6, A7): disclosed, unscored.
- Template changes; any wording of step 2 (parked for `/sprint-plan`).

## 7. Clarifications

| # | Date | Question | Resolution |
|---|---|---|---|
| C1 | 2026-10-12 | Real Status set? | Six values per `templates/campaign.md:5`; AC-2.4 requires a verdict for each |
| C2 | 2026-10-12 | One feature or two (B1, B2)? | One: same rule, same lines, same measurement setup (A4) |
| C3 | 2026-10-12 | Which ACs are best-effort, which session set carries the floor? | NFR-2; registered sets only, extras disclosed |
| C4 | 2026-10-12 | Clarify pass 1: scope, data, roles, edges, NFRs, integrations, out-of-scope | Edges: double notice (AC-1.2), absence line (AC-2.6), self-name collision (AC-2.5). Roles, integrations, UX states N/A |
| C5 | 2026-10-12 | Q1: line cap | **User:** `SKILL.md` ≤ 75 lines, every changed line ≤ 90 characters (NFR-1) |
| C6 | 2026-10-12 | Q2: floors and session sets | **User:** as proposed: registered 10 notice sessions floor 9 of 10; suppression 6 sessions floor 6 of 6; extras disclosed only (NFR-2) |
| C7 | 2026-10-12 | Q3: neutral absence line | **User:** allowed only when there is no other instance at all; forbidden when only `draft`/`halted`/`complete`/`abandoned` instances exist (AC-2.6) |
| C8 | 2026-10-12 | Q4: double notice | **User:** a miss for AC-1.1 under the strict count, lenient count disclosed beside it (AC-1.2) |
| C9 | 2026-10-12 | Q5: README scope | Not asked; default kept: rate figures only, after QA, from fresh numbers; `:48` only if its sole change is a figure (AC-3.2) |
| C10 | 2026-10-12 | Clarify pass 2: multiple other instances; Status-less, malformed or unreadable other instance; permissions; dependency failures | Multiple: per-instance rule, unscored check (A5). Unknown or unreadable Status: not announced as live, unscored (A6, A7; A7 open for the user's veto). Permissions: none, read-only. Unreadable `.spark/campaigns/` directory is refusal territory of the parent, not changed here. No high-impact ambiguity remains |

## 8. Design Review

N/A — not UI-facing: Markdown prompt material for a slash command; no screen or layout.

---

## ✅ SPEC GATE

- [x] Problem, goal and success signal are concrete (no buzzwords, no "everyone")
- [x] Every story has testable Given/When/Then acceptance criteria
- [x] Stories are prioritized (MoSCoW) and at least one is a Must
- [x] Non-functional requirements are stated and measurable (or marked N/A with reason)
- [x] Clarify pass done: no ambiguity left unresolved or unparked
- [x] Open questions are resolved or explicitly accepted as risk (A7 accepted with the approval)
- [x] Out-of-scope section is filled (something was consciously cut)
- [x] Constitution (`.spark/constitution.md`) respected, or conflicts recorded as open questions
- [x] Design review done for UI-facing features (or marked N/A with reason)
- [x] Line budget respected: Ist ~112 / Soll ~250 (excluding HTML comments) — self-reported, no linter checks this
- [x] Status set to `approved` by the user
