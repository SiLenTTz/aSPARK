# Spec: campaign-notice-line

| | |
|---|---|
| **Phase** | Specify |
| **Owner** | Product Owner (`/story-time`), Designer (`/look-and-feel`) |
| **Status** | `approved` |
| **Date** | 2026-10-16 |
| **Ticket** | none |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Summary:** four wordings of the step 2 notice rule traded "notice missed" for "non-live announced"; stop rewording, require the other-instance information as a line in the closing report that lists every other instance with its Status, measure fresh, state the dev-to-QA gap up front.
- **Open:** `0 open` - Q1-Q5 answered by the user (§3, §7); only the user's approval remains
- **Binding ruling:** §4 User Stories for the current stories; §7 Clarifications for what changed since the last round and why. User rulings C4-C8 are binding, C4 reverses the predecessor's "never announce non-live" story and C6 supersedes the predecessor's NFR-5.
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Problem & Goal

- **Problem:** `.spark/campaign-notice-statuses/release.md §4` and `qa.md`: wordings A-D2 of the step 2 rule moved one failure to the other pole. Shipped (v0.14.2): notice for a live instance 9 of 10 strict on the registered ten (5 of 10 for wording C, which had 9 of 10 in development); non-live instances never named 6 of 6, but strict-clean only 5 of 6, and `draft`/`halted` strict 3 of 6. The cause is placement: step 2 is mid-run, and the agent emits text almost only in the step 8 report (that report's fixed sentence was produced 39 of 39 and 40 of 40 in `campaign-draft-polish` QA). The user who runs `/campaign <name> [kind]` while other instances exist either misses a live clash or is told about finished campaigns as noise. System facts (stack, `SKILL.md` is the one file in play) from `constitution.md §9`; line count and step wording re-derived from source because this feature edits exactly that file (75 lines, step 2 at 27-34, step 8 at 65-69).
- **Goal:** the information about other instances reaches the user through the closing report of a written draft, in a form the strict count can decide, with a rate not below the floor (NFR-2) and no wording-by-wording trade between two poles.
- **Success signal:** fresh QA sessions against the installed plugin, n of m per AC, strict and lenient beside each other. No claim of "fixed" or "improved", only the observed rate.
- **Why now:** low urgency, said plainly. If never built: nothing breaks; the notice is advisory (one-active-campaign is not enforced). Reasons to build: two QA rounds were spent on wordings with no stable result; the structural change is the one untried lever. Displaces: any other `/campaign` work, nothing else.

## 2. Target Users

The maintainer or Claude Code user running `/campaign <name> [kind]` while other instances exist under `.spark/campaigns/` (the parent specs' user). Read-only consumer of the reply; no new role.

## 3. Assumptions & Open Questions

| # | Assumption / Question | Resolution |
|---|---|---|
| A1 | Original phrasing: "move the notice out of the step 2 prose into a required line in the step 8 closing report, listing other instances by Status". Underlying need: the user learns about other instances reliably, in the one place the agent speaks. Mechanism (which step holds which sentence) is parked for `/sprint-plan`. | Accepted |
| A2 | Prompt material makes the line likely, never certain. Best-effort ACs: AC-1.1, 1.2, 1.3, 2.1, 2.2. Checkable by reading: AC-2.3, AC-2.4. | Accepted, NFR-2 |
| A3 | A development matrix is not a QA forecast: wording C scored 9 of 10 in development and 5 of 10 in QA. Expected gap for this feature, stated before measuring: QA may land up to 4 of 10 below development. A development result at or above the floor is no evidence of meeting it. | Accepted, NFR-2 |
| A4 | The step 8 report exists only when a draft is written. Refusals (existing name, kind check, goal rejected) have no closing report and carry no other-instance line; the existing-name refusal still names that directory and its Status (parent AC-3.1). | Accepted default, unscored |
| A5 | Several other instances, and a Status outside the six or unreadable: one entry per instance, each once; an unrecognised or unreadable Status is shown as "Status not readable" and is never treated as live. Not measured by the floor sets; one disclosed unscored session (2 live + 1 `complete` + 1 malformed). Carried from `campaign-notice-statuses` A5-A7. | Accepted default, unscored |
| Q1 | Which instances does the line list? | Resolved (a), user ruling: every other instance with its Status, live ones (`approved`/`running`) distinguishable. Reverses the predecessor's "never announce non-live" story. C4 |
| Q2 | No other instance exists: does the line appear? | Resolved (a), user ruling: required, says "none". C5 |
| Q3 | Parent NFR-5 ("the rule sits in step 2") superseded? | Resolved (a), user ruling: step 2 only reads each other instance's Status; the emitting rule sits in step 8; this spec's NFR-5 supersedes the parent's. C6 |
| Q4 | Floors and sets as proposed in NFR-2? | Resolved yes, user ruling: notice 9/10 strict, listing 6/6 one per Status, no-instance 3/3, expected dev-to-QA gap up to 4 of 10, extras disclosed only, below floor one fix round then `refuted-with-finding`. C7 |
| Q5 | README rate figures updated from fresh QA numbers only? | Resolved yes (default, user did not object): figures only, `:48` only if its sole change is a figure. C8 |

## 4. User Stories

### US-1 (Must): Other instances are reported in the closing report

> As a user starting a new campaign draft, I want the closing report to tell me about the other campaign instances every time, so that I notice a possible clash before I approve anything.

**Acceptance criteria:**

- [ ] AC-1.1: Given `.spark/campaigns/` holds another instance at `approved` or `running`, when `/campaign <name> <kind> <goal>` writes a draft, then the final report names that instance and its Status and marks it as live, and the draft is still written. *Best-effort (NFR-2).*
- [ ] AC-1.2: Given the same fixture, when the whole reply is read, then the instance is named once in total, not in a mid-run message and again in the report. *Best-effort (NFR-2); a double mention is a miss for AC-1.1 under the strict count, the lenient count is disclosed beside it.*
- [ ] AC-1.3: Given no other instance exists, when a draft is written, then the final report states that there is none (Q2, user ruling C5). *Best-effort (NFR-2); checkable per session by presence.*

### US-2 (Must): The line is accurate for every Status

> As a user, I want every other instance listed with its true Status, so that a finished or parked campaign is shown as what it is and never mistaken for a clash. (User ruling C4: this replaces the predecessor's "non-live instances are never announced".)

**Acceptance criteria:**

- [ ] AC-2.1: Given the only other instance is at `draft`, `halted`, `complete` or `abandoned`, when a draft is written, then the report lists it with that Status and does not mark it live. *Best-effort (NFR-2).*
- [ ] AC-2.2: Given the only other instance has any one of the six Status values, then the report's Status for it equals the Status cell of its `campaign.md`, character for character. *Best-effort (NFR-2).*
- [ ] AC-2.3: Given `SKILL.md`, when it is read, then the rule for the line gives a verdict for each of the six Status values and for an unreadable one, none left to inference. *Checkable by reading, deterministic.*
- [ ] AC-2.4: Given the user types a name that already exists, then the refusal still names that directory and its Status (parent AC-3.1); no other-instance line is required there (A4). *Checkable by a refusal run.*

### US-3 (Must): No regression

> As a maintainer, I want the rest of `/campaign` to behave as before, so that moving one rule breaks nothing.

**Acceptance criteria:**

- [ ] AC-3.1: Given the change, when the kind check (7 of 7 key removals named, nothing written), Status `draft` / veto blank / no commit, existing-directory refusal, title `# Campaign: <name>` (`head -1`) and the closing sentence "This draft is not startable until these are filled and you approve." are re-run, then each shows no worse than `campaign-draft-polish` QA round 1 (7 of 7, 38 of 38, 4 of 4, 39 of 39, 40 of 40).
- [ ] AC-3.2: Given the change set, when `git diff --stat` is read, then only `skills/campaign/SKILL.md`, the best-effort rate figures in `campaigns/README.md` (Q5) and this feature's `.spark/campaign-notice-line/` files changed; `.spark/constitution.md`, `templates/`, `campaigns/*.md` kinds and `.claude-plugin/` are untouched (the release commit's version bump is outside the fence, as in v0.14.2).

## 5. Non-Functional Requirements

| # | Category | Requirement (measurable) | How it's verified |
|---|---|---|---|
| NFR-1 | Attention budget | `wc -l skills/campaign/SKILL.md` ≤ 75 (it is 75 now, so the change nets ≤ 0 lines), and no changed line over 90 characters (`awk 'length>90'` over the diff's added lines prints nothing); the 3 pre-existing 91-92 character lines are outside the diff. A higher count needs the user's recorded ruling. | `wc -l`, `awk`, /peer-review |
| NFR-2 | Best-effort rates (prompt material: likely, never certain) | **Only prompt material can make likely:** AC-1.1, 1.2, 1.3, 2.1, 2.2. **Floors, declared now (user ruling C7):** the *registered set* carries the floor. Notice set: 10 fresh sessions (5 `running`, 5 `approved`; 5 with no token budget; 1 hostile goal; every goal answered), floor 9 of 10 strict for AC-1.1/1.2. Listing set: 6 fresh sessions (one other instance each at `draft`, `approved`, `running`, `halted`, `complete`, `abandoned`), floor 6 of 6 strict for AC-2.1/2.2. No-instance set: 3 sessions, floor 3 of 3 for AC-1.3. **How extra sessions count:** development matrices, re-runs and pooled sessions are disclosed beside the registered sets, never used to claim improvement, to offset a registered miss or to lower a floor. **Expected dev-to-QA gap (A3):** up to 4 of 10 lower; the floor is not adjusted for it. Equal to the shipped 9 of 10 is "not worse", not an improvement. Below a floor: one fix round, then `refuted-with-finding` and the user decides; no further wording trade. Reports give n of m, strict and lenient; docs never call these guarantees. | Hands-on QA per constitution §8 (real `claude -p` sessions, performed steps only); /peer-review |
| NFR-3 | Library: public surface | No command, argument, output file name, template or protected structure (constitution §3) renamed, added or removed; `/campaign <name> [kind]` unchanged. | /peer-review |
| NFR-4 | Library: compatibility | Instances written by the previous version stay valid and untouched (`shasum` before = after); the draft written has the same shape; only the report gains the line. | Recorded dry run + /peer-review |
| NFR-5 | Library: contract clarity | By user ruling (C6) step 2 only reads each other instance's Status; the rule that makes the line appear sits in the step that emits the report (step 8), the step open when the agent speaks (CLAUDE.md "Put a rule in the file the agent actually reads"). This supersedes the parent's NFR-5 ("rule in step 2"). `SKILL.md` still states input, output, what is never written and each refusal. README figures keep "best effort, not a guarantee" and state registered and pooled n of m separately. | /peer-review |
| NFR-6 | Stack / approval | Zero new tracked files beyond this spec set; no executable code; `claude plugin validate .` passes; constitution unchanged; nothing committed or approved by the command. Diff fence: AC-3.2. | /peer-review |
| NFR-7 | Negative case first | In a repo that never runs `/campaign` (`/spark`, `/next-steps`, a plain prompt) no ceremony output differs; run first. | Recorded dry run |

Active lens `library`: public surface, compatibility, contract clarity are NFR-3, NFR-4, NFR-5; packaging is N/A (no bundle, no dependencies). Other lenses off (constitution §2).

## 6. Out of Scope

- Constitution F8 and any constitution edit (a `/charter` matter).
- `campaigns/README.md:48` unless its only needed change is a figure; any other doc.
- Running, resuming or routing campaigns; new kinds; a second command.
- Enforcing one-active-campaign or blocking on a clash; the notice stays advisory.
- Making the line deterministic: impossible in prompt material; improvement is measured, never promised.
- Scored floors for several other instances or malformed ones (A5): disclosed, unscored.
- Other open Minors of `campaign-draft-polish` (B3 dropped kind heading, B4 kind check order, B5 tool set).
- Any wording or placement detail of the line (parked for `/sprint-plan`).
- Suppressing or filtering non-live instances in the line (reversed by C4); a separate "count of the rest" form (Q1 option c, not chosen).

## 7. Clarifications

| # | Date | Question | Resolution |
|---|---|---|---|
| C1 | 2026-10-15 | Why not another wording? | Four wordings (A-D2) swapped poles; the lever left is where the text is emitted (release.md §4) |
| C2 | 2026-10-15 | Which ACs are best-effort, which sessions carry the floor, how do extras count, what gap is expected? | NFR-2 and A3: registered sets only, extras disclosed, gap up to 4 of 10 |
| C3 | 2026-10-15 | Clarify pass: scope, data, roles, edges, NFRs, integrations, out-of-scope | Edges: refusals have no report (A4), double mention (AC-1.2), none-exist (Q2), malformed (A5). Roles, integrations, UX states N/A (read-only, Markdown) |
| C4 | 2026-10-16 | Q1: which instances does the line list? | User ruling (a): every other instance with its Status, live ones distinguishable. Deliberately reverses the predecessor's "never announce non-live" story; US-2, AC-2.1/2.2 rewritten, suppression leaves scope |
| C5 | 2026-10-16 | Q2: no other instance? | User ruling (a): line required, says "none" (AC-1.3) |
| C6 | 2026-10-16 | Q3: where does the rule sit? | User ruling (a): step 2 only reads Status; emitting rule in step 8; NFR-5 here supersedes the parent's NFR-5 |
| C7 | 2026-10-16 | Q4: floors and sets? | User ruling: as proposed in NFR-2 (9/10, 6/6, 3/3, gap up to 4 of 10, extras disclosed only, one fix round then `refuted-with-finding`) |
| C8 | 2026-10-16 | Q5: README figures? | Default, user did not object: fresh QA figures only; `:48` only if its sole change is a figure |
| C9 | 2026-10-16 | Clarify pass 2 after C4-C8: boundaries, data, permissions, errors, NFRs, integrations, UX states, out-of-scope | No high-impact ambiguity left. Live = `approved`/`running` only (AC-1.1, AC-2.1); AC-2.2 widened to all six Statuses so listing accuracy is scored for every Status; large instance counts and malformed Status stay unscored (A5). Permissions, integrations, UX states N/A |

## 8. Design Review

N/A — not UI-facing: Markdown prompt material for a slash command; no screen or layout.

---

## ✅ SPEC GATE

- [x] Problem, goal and success signal are concrete (no buzzwords, no "everyone")
- [x] Every story has testable Given/When/Then acceptance criteria
- [x] Stories are prioritized (MoSCoW) and at least one is a Must
- [x] Non-functional requirements are stated and measurable (or marked N/A with reason)
- [x] Clarify pass done: no ambiguity left unresolved or unparked (C9)
- [x] Open questions are resolved or explicitly accepted as risk (Q1-Q5 resolved, C4-C8)
- [x] Out-of-scope section is filled (something was consciously cut)
- [x] Constitution (`.spark/constitution.md`) respected, or conflicts recorded as open questions (NFR-5 supersession ruled by the user, C6)
- [x] Design review done for UI-facing features (or marked N/A with reason)
- [x] Line budget respected: Ist ~112 / Soll ~250 (excluding HTML comments) — self-reported, no linter checks this
- [x] Status set to `approved` by the user
