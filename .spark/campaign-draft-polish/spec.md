# Spec: campaign-draft-polish

| | |
|---|---|
| **Phase** | Specify |
| **Owner** | Product Owner (`/story-time`), Designer (`/look-and-feel`) |
| **Status** | `approved` |
| **Date** | 2026-10-09 |
| **Ticket** | none |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Summary:** `/campaign` drafts still miss three small, accepted details (placeholder title, other-instance notice, closing sentence); make each hold more reliably, measured fresh, without growing `SKILL.md`.
- **Open:** `none` — Q1-Q5 resolved by the user (§7 C6-C10); approved by the user 2026-10-09
- **Binding ruling:** §4 User Stories for the current stories; §7 Clarifications for what changed since the last round and why
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Problem & Goal

- **Problem:** `.spark/campaign-start/qa.md` round 2 left three Minors open, all accepted: B4 (5 of 16 drafts keep the title `# Campaign: <campaign-name>`), B6 (AC-3.3 notice about other `approved`/`running` instances omitted in 1 of 7 drafts), B8 (AC-1.7 closing sentence not verbatim in 1 of 5 end reports). The user who runs `/campaign` sees a draft whose title looks unfilled, an invisible clash with a live campaign, or a closing line that differs run to run. System facts (11 skills, `/campaign` run by the main session) from `constitution.md §9`; the over-cap fact (75 lines vs 70) re-derived from source because this feature edits exactly that file.
- **Goal:** the three behaviors hold at a measured rate at least as good as, and for B4 better than, the round-2 baseline, in the same single file, with no regression elsewhere.
- **Success signal:** fresh QA sessions against the installed plugin, reported as n of m per AC (§5 NFR-2). No claim of "fixed", only of the observed rate.
- **Why now:** low urgency, said plainly. If never built: nothing breaks; the path already carries the name, the notice is advisory (A6 of the parent: one-active-campaign is not enforced anyway), the substance of the closing line holds 5 of 5. The only live reason is that the three misses are the loop's own recorded, user-accepted debt, and B4 exposes a real ambiguity in `SKILL.md` step 7 (see A1).

## 2. Target Users

The maintainer or a Claude Code user running `/campaign <name> [kind]` to start a campaign draft (parent spec's user). Read-only consumer of the draft title and the end report; no new role.

## 3. Assumptions & Open Questions

| # | Assumption / Question | Resolution |
|---|---|---|
| A1 | B4 is a defect only because it is inconsistent: step 7 says "change only" the Kind row, Status, answers and `SR-5…` row, so the title is outside what the agent may change, yet 11 of 16 drafts changed it. The template placeholder is not deliberate for instances (the template says "Copy this file to `.spark/campaigns/<campaign-name>/campaign.md`"). Original phrasing: "the draft's title keeps the placeholder". | Confirmed by the user (Q1, C6) |
| A2 | B6 and B8 baselines (1 of 7, 1 of 5) are too small to show a fix. Honest sample: see NFR-2. | Accepted risk, stated in NFR-2 |
| A3 | The failing sessions cluster: 4 of the 5 B4 misses and the B6 miss were in the other-instance scenarios (r-o*), B8's in the no-token-budget scenario (r-h2). Fresh runs must include those scenarios or the rates are not comparable. | Folded into NFR-2 |
| A4 | The README rate sentences are updated only after QA has measured, from the fresh numbers, by the Act/Keep step, not by hand-estimate. Who edits and when is a `/sprint-plan` matter. | Accepted |
| Q1-Q5 | Resolved by the user 2026-10-09, see §7 C6-C10. | closed |

## 4. User Stories

### US-1 (Must): Other-instance notice is always given

> As a user starting a new campaign draft while another is `approved` or `running`, I want to be told about it every time, with that instance's Status, so that I notice a possible clash before I approve anything.

**Acceptance criteria:**

- [ ] AC-1.1: Given `.spark/campaigns/` holds an instance at `approved` or `running`, when `/campaign <name> <kind> <goal>` writes a draft, then the reply names that instance and its Status exactly once, and the draft is still written. *Best-effort (NFR-2).*
- [ ] AC-1.2: Given no other instance is `approved` or `running` (only `draft`, `halted`, or none), when a draft is written, then no such notice appears. (Suppression: no new false notice.)

### US-2 (Should): The draft title names the campaign

> As a user opening a fresh draft, I want the title to carry my campaign's name, so that the file does not look unfilled.

**Acceptance criteria:**

- [ ] AC-2.1: Given a written draft, when `head -1` of `.spark/campaigns/<name>/campaign.md` is read, then it equals `# Campaign: <name>` with the real name and no `<` or `>`. *Best-effort (NFR-2).*
- [ ] AC-2.2: Given the same draft, when it is diffed against `templates/campaign.md` plus the kind body, then the title line is the only added difference beyond what step 7 already permits (Kind row, Status cell, answers, `SR-5…` row, `## 8`); the template's HTML comment and every unfilled line are still verbatim. (Does not weaken "copy verbatim".)

### US-3 (Could): Closing sentence is verbatim

> As a user reading the end report, I want the same closing sentence every time, so that I can recognise "not startable yet" at a glance.

**Acceptance criteria:**

- [ ] AC-3.1: Given a draft with at least one placeholder left, when the end report is shown, then it contains the exact phrase "not startable until these are filled and you approve". *Best-effort (NFR-2).*

### US-4 (Must): No regression

> As a maintainer, I want the rest of `/campaign` to behave as before, so that fixing three details breaks nothing.

**Acceptance criteria:**

- [ ] AC-4.1: Given the fix, when the parent's AC-1.4 (kind check, malformed kind refused), AC-1.6 (Status `draft`, veto blank, no commit), AC-3.1 (existing directory refused) and the AC-1.7 placeholder list and next-step naming are re-run, then each shows no worse than its recorded round-2 rate (24 of 24 for the kind check, per `campaigns/README.md`).
- [ ] AC-4.2: Given the change set, when `git diff --stat` is read, then only `skills/campaign/SKILL.md`, the best-effort rate sentences in `campaigns/README.md` (rates only, nothing else in that file) and this feature's `.spark/campaign-draft-polish/` files changed; `.spark/constitution.md` is untouched.

## 5. Non-Functional Requirements

| # | Category | Requirement (measurable) | How it's verified |
|---|---|---|---|
| NFR-1 | Attention budget | `wc -l skills/campaign/SKILL.md` ≤ 75 after the change (net zero or fewer; the 70 cap of the parent NFR-1 is not restored here). A higher count requires the user's recorded ruling (C8), not the implementer's. | `wc -l` + /peer-review |
| NFR-2 | Best-effort rates | AC-1.1, AC-2.1, AC-3.1 are prompt material: likely, never certain. Rates are measured fresh by QA against the installed plugin, never carried from `campaign-start`, as n of m, with at least 10 sessions per AC, including the failing scenarios of A3 (other instance at `running` and at `approved`; no token budget; draft with answered goal). Floor: 9 of 10 for each (baselines 6 of 7, 4 of 5 and 11 of 16; the parent's 3-of-5 floor would prove nothing). Below the floor: one fix round, then `refuted-with-finding` and the user decides. A clean 10 of 10 still means only "no miss seen in 10". Docs never describe these as guarantees. | Hands-on QA per constitution §8 (performed steps only); /peer-review |
| NFR-3 | Library: public surface | No command, argument, output file name, template or protected structure (constitution §3) renamed, added or removed; `/campaign <name> [kind]` unchanged. | /peer-review |
| NFR-4 | Library: compatibility | Instances written by the previous version stay valid and untouched; a draft from this version is read the same way by the kind-check, the template contract and `aspark-graph`. | Recorded dry run on an old instance + /peer-review |
| NFR-5 | Library: contract clarity | Where a behavior is reworded, `SKILL.md` still states input, output file, what is never written and each refusal; the rule sits in the step the agent is executing when it must act (CLAUDE.md "Put a rule in the file the agent actually reads"). The updated README rate sentences state the new n of m and keep the "best effort, not a guarantee" wording. | /peer-review |
| NFR-6 | Stack / approval | Zero new tracked files beyond this spec set; no executable code; `claude plugin validate .` passes; constitution unchanged (F8 stays a `/charter` matter); the command still sets nothing beyond `draft` and commits nothing. Diff fence: AC-4.2. | /peer-review |
| NFR-7 | Negative case first | In a repo that never runs `/campaign`, no ceremony output differs; run first. | Recorded dry run |

Active lens `library`: public surface, compatibility, contract clarity are NFR-3, NFR-4, NFR-5; packaging is N/A (no bundle, no dependencies). Other lenses off (constitution §2).

## 6. Out of Scope

- Constitution F8 (a `/charter` matter) and any constitution edit.
- Running, resuming or routing campaigns; new kinds; a second command.
- Any other `campaign-start` Minor (B5, B7) and any new check; enforcing one-active-campaign (parent A6).
- Making the three checks deterministic: impossible in prompt material; improvement is measured, never promised.
- Template changes (`templates/campaign.md` title placeholder stays; the instance, not the template, carries the name).
- Any edit to `campaigns/README.md` beyond the best-effort rate sentences; any other doc.
- Splitting into three features: rejected, the three share one file, one measurement setup and one cap.

## 7. Clarifications

| # | Date | Question | Resolution |
|---|---|---|---|
| C1 | 2026-10-07 | Is B4 deliberate? Decidable condition? Conflict with "verbatim"? | Not deliberate for instances, but unaddressed in step 7; condition is `head -1` equal to `# Campaign: <name>` (AC-2.1); AC-2.2 keeps the rest verbatim |
| C2 | 2026-10-07 | What does the 70-line cap do here? | Requirement only: ≤ 75; how is for `/sprint-plan` (NFR-1) |
| C3 | 2026-10-07 | Which ACs are best-effort and what sample is honest? | AC-1.1, AC-2.1, AC-3.1; 10 sessions each, floor 9 of 10, fresh rates (NFR-2, A2, A3) |
| C4 | 2026-10-07 | One feature or three; drop any? | One; B6 the only Must (withholds information) |
| C5 | 2026-10-07 | Regression, new files, constitution | AC-4.1, AC-4.2, NFR-6 |
| C6 | 2026-10-09 | Q1: fix B4 or accept? | **User:** fix; B4 stays Should (A1 confirmed) |
| C7 | 2026-10-09 | Q2: keep or drop B8? | **User:** keep as Could (US-3) |
| C8 | 2026-10-09 | Q3: cap yield or net zero? | **User:** net zero, `SKILL.md` ≤ 75; above 75 needs the user's recorded ruling, not the implementer's (NFR-1) |
| C9 | 2026-10-09 | Q4: update README rates? | **User:** yes, rates-only edits to the best-effort rate sentences of `campaigns/README.md` (~lines 22 and 48) after QA, nothing else there; fence widened in AC-4.2, NFR-6, §6 |
| C10 | 2026-10-09 | Q5: sample size | **User:** 10 fresh sessions per AC, floor 9 of 10; below it one fix round, then refuted-with-finding and the user decides (NFR-2) |
| C11 | 2026-10-09 | Clarify pass over scope, data, edges, NFRs, out-of-scope | No further high-impact ambiguity. Edges covered: no notice for `draft`/`halted` instances (AC-1.2); refusals write nothing so the title rule never applies to them; a fixed title is no longer a placeholder, so AC-1.7's list must not name it (checked in AC-4.1); technical choices parked for `/sprint-plan` |

## 8. Design Review

N/A — not UI-facing: the change is Markdown prompt material for a slash command; there is no screen, layout or interaction to review.

---

## ✅ SPEC GATE

- [x] Problem, goal and success signal are concrete (no buzzwords, no "everyone")
- [x] Every story has testable Given/When/Then acceptance criteria
- [x] Stories are prioritized (MoSCoW) and at least one is a Must
- [x] Non-functional requirements are stated and measurable (or marked N/A with reason)
- [x] Clarify pass done: no ambiguity left unresolved or unparked
- [x] Open questions are resolved or explicitly accepted as risk
- [x] Out-of-scope section is filled (something was consciously cut)
- [x] Constitution (`.spark/constitution.md`) respected, or conflicts recorded as open questions
- [x] Design review done for UI-facing features (or marked N/A with reason)
- [x] Line budget respected: Ist ~120 / Soll ~250 (excluding HTML comments) — self-reported, no linter checks this
- [x] Status set to `approved` by the user
