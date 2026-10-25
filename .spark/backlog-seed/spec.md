# Spec: backlog-seed

| | |
|---|---|
| **Phase** | Specify |
| **Owner** | Product Owner (`/story-time`) |
| **Status** | `approved` |
| **Date** | 2026-10-25 |
| **Ticket** | #81 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Summary:** New `/backlog` skill: Product Owner turns `docs/` into an ordered, user-edited list of feature-sized items, filed as GitHub issues on explicit confirmation.
- **Open:** `none`
- **Binding ruling:** §4 User Stories; §7 Clarifications for decisions taken.
- **On conflict:** the numbered body wins for everything except `Status`.

## 1. Problem & Goal

- **Problem:** A new repo with documents (vision, roadmap, specs in `docs/`) but no backlog gives the loop nothing to start from. `/next-steps` proposes one feature in chat and writes nothing; external consumers of a backlog need issues, not chat text. Seeded from issue #81.
- **Goal:** One ceremony that reads the project documents, proposes an ordered backlog, lets the user edit it, and files it as issues that `/story-time <n>` can import.
- **Success signal:** On a repo with `docs/`, `/backlog` ends with N issues filed, each traceable to a `docs/` section and importable by `/story-time <n>`.
- **Why now:** The empty-repo path ends at "bring me an idea".

## 2. Target Users

Maintainers starting a project from documents; teams or unattended runners that consume labelled issues.

## 3. Assumptions & Open Questions

| # | Assumption / Question | Resolution |
|---|---|---|
| A1 | New skill vs. extending `/next-steps` | New skill `/backlog` (user decision, 2026-10-25); `/next-steps` unchanged |
| A2 | Repo is resolved from `origin` as in `/story-time` | Accepted |
| A3 | Label config location | Optional `.spark/backlog.yml` with a `labels:` list |

## 4. User Stories

### US-1 (Must): Read the sources
> As a maintainer, I want `/backlog` to read `docs/`, `.spark/constitution.md` and `ROADMAP.md`, so proposals come from stated goals.

- [ ] AC-1.1: Given `docs/` exists, when `/backlog` runs, then it reads `docs/` plus the constitution and `ROADMAP.md` if present.
- [ ] AC-1.2: Given neither `docs/` nor a constitution, when it runs, then it points to `/charter` first, invokes nothing and writes nothing.

### US-2 (Must): Proposed ordered list
> As a maintainer, I want an ordered list of feature-sized items with traceable sources.

- [ ] AC-2.1: Each item has title, short problem statement, size (S/M/L), dependencies and a source reference `docs/<file>#<section>`.
- [ ] AC-2.2: An item without a source reference is not proposed (nothing invented).
- [ ] AC-2.3: Dependencies refer to other items in the list.

### US-3 (Must): Edit before writing
> As a maintainer, I want to drop, merge, reorder and reword items before anything is written.

- [ ] AC-3.1: The list is shown as a plan and no issue is created before explicit confirmation of the final list in this conversation.
- [ ] AC-3.2: Edits are applied and the list re-shown until confirmed or abandoned; abandoning writes nothing.

### US-4 (Must): File as issues
> As a maintainer, I want confirmed items filed as GitHub issues that `/story-time <n>` can import.

- [ ] AC-4.1: Each item is filed via `gh issue create --repo <owner/repo>`, the repo resolved once from `origin` and echoed before filing.
- [ ] AC-4.2: The body holds problem, size, dependencies (as `#n` once the issue exists, else by title), source reference and a line `generation: 0`.
- [ ] AC-4.3: If `gh`/`origin` fails, stop, report the real error; no silent partial success: report which issues were created.

### US-5 (Should): Optional labels
- [ ] AC-5.1: Default is no label.
- [ ] AC-5.2: Labels come from user-passed arguments, else from optional `.spark/backlog.yml` (`labels: [..]`); user arguments win.
- [ ] AC-5.3: Nothing in the skill names any particular automation.

## 5. Non-Functional Requirements

| # | Category | Requirement | Verified |
|---|---|---|---|
| NFR-1 | Safety | Only network/write step is `gh issue create` after confirmation; no `.spark/` files written | /peer-review |
| NFR-2 | Docs | README, workflow docs and `/spark` empty-state mention the new ceremony; counts of ceremonies updated | /peer-review |
| NFR-3 | Honesty | Prompt-material checks (e.g. source references) are instructions, not enforcement; documented as such | /peer-review |

## 6. Out of Scope
Writing `.spark/` specs per item; external-tool integration; deduplicating against existing issues; follow-up generation logic (only the `generation: 0` marker).

## 7. Clarifications

| # | Question | Answer |
|---|---|---|
| C1 | New skill or `/next-steps` extension? | New skill `/backlog` |
| C2 | Label config format | `.spark/backlog.yml`, single `labels:` list |

## SPEC GATE
- [ ] Stories have testable ACs · [ ] No open questions · [ ] Not UI-facing (design review N/A: Markdown prompt material)
