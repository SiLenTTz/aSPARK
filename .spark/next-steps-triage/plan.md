# Plan: next-steps-triage

| | |
|---|---|
| **Phase** | Plan |
| **Owner** | Engineering Manager (`/sprint-plan`) |
| **Input** | `.spark/next-steps-triage/spec.md` (`approved`) |
| **Status** | `approved` |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** mirrors the header table above (authoritative for `Status`).
- **Summary:** Keep the drafted prompt change (commit `64767bc`); verify it against AC-1..AC-6 with fixture sessions (negative case first), fix the wording defects found at plan time, document the Factory JSON block, and check every place that dispatches or describes `/next-steps`.
- **Open:** T2, T4 `skipped`; T1, T3, T5, T7 `partial` (§3, corrected at review r1); Q1 and Q2 ruled by the agent, not the user (review F1); Q3, Q4 open for the PO
- **Binding ruling:** §3 Task Breakdown for current task status; a plan revision after review/QA findings updates §1/§3 in place, never a new section
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Architecture Decision

- **Context:** Core is Markdown + JSON only, no runtime and no test suite (`constitution.md §3`, `§4`). `/next-steps` delegates to `agents/product-owner.md` (`constitution.md §9`, Stack & entry points); the skill gathers the brief because the agent has no shell. The change is already drafted in `skills/next-steps/SKILL.md:69-116,127-134` and `agents/product-owner.md:169-180`. The spec's ACs live in issue #110, not in the repo; the inline `(AC-n)` tags in `SKILL.md` give AC-2 (blocked → blocker), AC-3 (gaps), AC-4 (merged PR → "done, close it"), AC-5 (tickets unreachable). AC-1 (greenfield → foundation) and AC-6 (Factory `existing_issue`) are inferred from the spec's Change line and the commit subject. §9 facts used as given; the `/next-steps` area itself was re-read from source because the feature touches it (condition a).
- **Decision:** No new file, skill, agent or mode. Treat the drafted text as the implementation and spend the increment on (1) a static wording check with the defects below fixed in the same two files, (2) real fixture sessions with the PO agent and the skill, negative case first, (3) one docs paragraph that defines the JSON block as a consumed contract, (4) a dispatch check of every mention. Wording defects found while planning, to be fixed in T1:
  - D1 `SKILL.md:91-95` step 2 hands the agent a closed list (shipped, in-flight/stalled, constitution) that omits maturity class, triage result, gaps and ticket reachability, so the agent's step 6 has nothing to work from (CLAUDE.md "Put a rule in the file the agent actually reads").
  - D2 `SKILL.md:65-67` cites the PO's "step 6" for the thin-brief rule; that rule is now step 7 (`product-owner.md:181`).
  - D3 `product-owner.md:169` says "Classify maturity **first**" but sits after step 5, which already returns the ranking.
  - D4 When tickets are unreachable, `SKILL.md:75` says "behave exactly as without this bullet", but the triage and gap bullets (`:80-90`) are not told to skip; with no issue list every function looks like a gap.
- **Alternatives considered:**
  | Alternative | Why rejected |
  |---|---|
  | New `/triage` skill or a separate PO mode | Splits one recommendation over two ceremonies; the user asked for `/next-steps` to read tickets. A new ceremony also bumps the counts in `constitution.md §2/§9` |
  | Verify by reading the Markdown only, no sessions | `constitution.md §8`: reasoning about a file is never a performed step; `§4` requires a dogfood, negative case first |
  | Emit the JSON block only when `agent.yaml` exists (Factory repo marker) | Possibly right (see Q2), but it changes the drafted behaviour; not picked silently — the user rules on Q2 |
  | Ship `existing_issue` in the Factory repo only | The field is produced here; an undocumented output key is a silent contract (`library` lens, `constitution.md §2`) |
- **Consequences:** Easier: the Factory can reuse an issue instead of filing a duplicate; recommendations trace to tickets and sources. Harder: every `/next-steps` run now costs `gh` calls and a longer brief; the JSON keys become a cross-repo contract with no version handshake, like the template contract (`constitution.md §3`). Behaviour stays best effort; rates go into the docs, never "the agent does X".

## 2. Affected Components

- `skills/next-steps/SKILL.md`, `agents/product-owner.md` — wording fixes D1-D4 and the Q1/Q2 rulings.
- `docs/workflow.md` — JSON block contract next to the `/backlog` paragraph (`:30-34`).
- `README.md:106` (command row), `docs/status.md` (new section with observed rates), `ROADMAP.md` (Shipped row).
- Dispatch sites, checked and fixed only if wrong: `skills/spark/SKILL.md:32-35,125`, `skills/charter/SKILL.md:34`, `skills/backlog/SKILL.md:35`, `agents/product-owner.md:7,133-135`, `README.md:76`, `docs/workflow.md:24`, `campaigns/README.md:72`, `.github/ISSUE_TEMPLATE/field_report.yml:36`.
- Not touched: `agent.yaml` (Factory-protected path, `agent.yaml:37`), `.claude-plugin/plugin.json` (minor bump belongs to `/go-live`, `constitution.md §5`), Factory's `proposals.py` (other repo, spec AC-6).
- No new dependency. `gh` is read-only and optional; absence must degrade (AC-5, Q1).
- Blast radius: no tool file passed; scoped by hand from `Grep` over the repo for `next-steps|existing_issue|Factory`.

## 3. Task Breakdown

Story column: the spec has no `US-` IDs; tasks trace to issue #110 and its AC-1..AC-6 as the spec names them.

| # | Task | Story | Covers (AC / NFR) | Depends on | Status | Definition of Done |
|---|---|---|---|---|---|---|
| T1 | Static wording check and fixes D1-D4, plus the user's Q1/Q2 rulings | #110 | AC-1, AC-2, AC-3, AC-4, AC-5, AC-6 | – | `partial` (review r1: D1-D4 fixed and Q1/Q2 applied, but `evidence.md` has no per-AC `file:line` rows and Q1/Q2 were ruled by the agent, not the user) | `evidence.md` has one row per AC citing the `file:line` that carries it; D1-D4 each fixed or ruled with reason; step numbers 1-5 in the PO section unchanged; `grep -n "step 6" skills/next-steps/SKILL.md` no longer points at the thin-brief rule — files: skills/next-steps/SKILL.md, agents/product-owner.md, .spark/next-steps-triage/evidence.md |
| T2 | Negative case first: real `/next-steps` in an untracked scratch repo with a constitution and no remote, 3 sessions | #110 | AC-5, AC-6 | T1 | `skipped` (review r1: not run, no remote and no `gh` in the autonomous run, `evidence.md:12`; AC-5 checked by reading only) | `evidence.md` records 3 sessions: k/3 with no triage and no gap list, k/3 with the unreachable note exactly once in the closing report (or absent, per Q1), k/3 whose JSON parses and has no `existing_issue` — files: .spark/next-steps-triage/evidence.md |
| T3 | Fixture briefs F1-F4 to the PO agent in propose mode, 3 sessions each (fixtures below) | #110 | AC-1, AC-2, AC-3, AC-4, AC-6 | T2 | `partial` (review r1: 1 session per fixture, not 3, `evidence.md:3`; F1 with open #1 and F1b not run as specified; fixture briefs and raw output not quoted) | `evidence.md` has a k/3 per fixture against its expected outcome, with the raw first line and JSON of each session quoted; any 0/3 recorded as refuted-with-finding — files: .spark/next-steps-triage/evidence.md |
| T4 | One positive end-to-end `/next-steps` on this repo (read-only `gh issue list` / `gh pr list` on a-lottes/aSPARK) | #110 | AC-2, AC-3, AC-4, AC-6 | T3 | `skipped` (review r1: live run not performed, `evidence.md:12`) | `evidence.md` shows the `gh` commands ran, the maturity class with its evidence, a typed first line, and a JSON block that parses; no issue was closed or filed — files: .spark/next-steps-triage/evidence.md |
| T5 | Dispatch check of every site in §2 | #110 | AC-5, AC-6 | T1 | `partial` (review r1: no per-site rows in `evidence.md`; the reviewer's own check found all 9 sites correct, review §2) | `evidence.md` has one row per site: `correct`, `fixed` or `stale, routed`; any fix is in the listed file and nothing outside §2 changed — files: .spark/next-steps-triage/evidence.md |
| T6 | Document the Factory JSON block as a contract | #110 | AC-6 | T1 | `done` | `docs/workflow.md` names the five keys, their types, that `existing_issue` is an integer present only when `type` is `existing`, that parsing lives in aSPARK-Factory, and that there is no version handshake; the text matches `SKILL.md` step 4 word for word on key names — files: docs/workflow.md |
| T7 | Docs in step with measured rates | #110 | AC-1, AC-2, AC-3, AC-4, AC-5, AC-6 | T2, T3, T4, T6 | `partial` (review r1: `README.md:106` updated; no `docs/status.md` section, no `ROADMAP.md` row, no `claude plugin validate` output in `evidence.md`) | `README.md:106` mentions ticket triage; `docs/status.md` has a section whose rates equal `evidence.md` and whose State says best effort/unproven where below 3/3; `ROADMAP.md` has a Shipped row with the same labels; `claude plugin validate .` passes and its output is in `evidence.md` — files: README.md, docs/status.md, ROADMAP.md, .spark/next-steps-triage/evidence.md |

**Fixtures (T3).** Each is a written brief handed to the PO agent, kept in `evidence.md`.
- F1 greenfield (AC-1): constitution with stack and quality bars, 3 tracked files, issues #1 "Set up test harness" (open) and #2 "Add login" (open). Expect `existing #1`, foundation work before #2. F1b: same without #1 → `foundation`.
- F2 blocked (AC-2, AC-6): #5 "Export report" (open, "blocked by #4"), #4 "Report data model" (open). Expect `existing #4`; JSON `"type": "existing", "existing_issue": 4`.
- F3 gaps (AC-3, AC-6): constitution §4 names CI, closed #8 covers CI; `ROADMAP.md` Next names "audit log", no issue. Expect "audit log" listed as a gap with its source; CI not a gap; if the pick is a gap, JSON has no `existing_issue`.
- F4 merged PR (AC-4): open #7 with merged PR "Closes #7". Expect #7 under "done, close it", never recommended, nothing closed.

## 4. Test Strategy

No unit or integration tests exist or are possible (`constitution.md §4`). Verification is real ceremony and agent invocations against the installed plugin (`§8`), negative case first (T2), then fixtures (T3), then one live read-only run (T4). Session counting, decided now: the **registered set** is 3 sessions per fixture recorded in `evidence.md`; that set alone gives each AC its rate. Extra sessions are disclosed separately and never pooled to raise a figure. Verdict per AC: 3/3 confirmed, 1-2/3 partial with the rate written into `docs/status.md`, 0/3 refuted-with-finding. No pass floor is promised; a dev-time rate is not a QA forecast. `/demo-day`: no browser surface (`§8`); QA reruns T2 and F2 against the installed plugin.

## 5. Risks & Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| **Q1 (constitution conflict).** `SKILL.md:74-75,107-108` says "Could not see the tickets" once when no ticket tool works; `constitution.md §6` says an optional integration degrades to silence, "no error, no warning, no mention" | A Non-negotiable broken by AC-5 as drafted | User rules before approval: drop the note, or keep it with a recorded `/charter` amendment or waiver. T1 applies the ruling |
| **Q2 (constitution conflict).** The JSON block is emitted on every run, also for users without the Factory (`§1` suppression, `§6` silence) | Noise in every interactive report | User rules: always emit (record why), or emit only when `agent.yaml` exists or the caller asks. T1 applies it |
| **Q3 (spec, for the PO).** Spec header `Ticket: issue #110` vs `constitution.md §7` ticket format `none` and tracker-handoff NFR-10; spec also has no AC text in the repo | Traceability depends on an external issue; AC-1 and AC-6 here are inferred | PO confirms AC-1..AC-6 match §1's reading, and rules on the Ticket row. The plan does not edit the spec |
| **Q4 (spec, for the PO).** Spec Disclosure says no fixture sessions run in this loop; T2-T4 run them | Disclosure goes stale once T2 lands | PO updates the Disclosure with the T2-T4 rates after T7 |
| Prompt material: agent skips `gh`, mistypes the first line, or adds `existing_issue` to a gap | Factory files a duplicate or works the wrong issue | Rates measured and written down (T7); Factory must treat a missing or non-integer `existing_issue` as absent (T6 text) |
| `gh` rate limits or auth prompts in T4 | Live run stalls | Read-only commands only, `--limit 200`; a failed call is recorded, not retried into success |
| JSON keys become a cross-repo contract with no version marker | A rename later silently breaks the Factory | T6 documents the keys; adding them to `constitution.md §3`'s protected list is a `/charter` follow-up, flagged in the release |
| Scratch repos for T2 leaking into the working tree (`constitution.md §3`, published surface) | Junk shipped to every consumer | Scratch repos live outside `/workspace/repo`; only `evidence.md` is committed |

---

## ✅ PLAN GATE

*All boxes checked → `/increment` may start. Any box open → back to `/sprint-plan`.*

- [x] Spec status is `approved` (never plan against a draft)
- [x] Architecture decision includes rejected alternatives (a decision without alternatives is a guess)
- [x] Architecture respects the constitution (Q1 and Q2 ruled, see evidence.md)
- [x] Every task maps to a user story — no orphan tasks, no story without tasks (story = issue #110; spec has no `US-` IDs)
- [x] Every Must AC and every applicable NFR is covered by at least one task (AC-1..AC-6; the spec has no NFRs)
- [x] Every task has a checkable definition of done
- [x] Task order respects dependencies
- [x] Test strategy covers every Must story
- [x] Line budget respected: Ist 97 / Soll ~300 (excluding HTML comments)
- [x] Status set to `approved` (autonomous mode, recommended option)
