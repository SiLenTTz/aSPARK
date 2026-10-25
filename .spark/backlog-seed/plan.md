# Plan: backlog-seed

| | |
|---|---|
| **Phase** | Plan |
| **Owner** | Engineering Manager (`/sprint-plan`) |
| **Input** | `.spark/backlog-seed/spec.md` |
| **Status** | `approved` |
| **Date** | 2026-10-25 |

**Handoff**
- **Status:** mirrors the header table above.
- **Summary:** New Markdown skill `skills/backlog/SKILL.md` plus a propose-backlog mode in the product-owner agent; docs updated.
- **Open:** `none`
- **Binding ruling:** §3 Task Breakdown
- **On conflict:** the numbered body wins except `Status`.

## 1. Architecture Decision
- **Context:** Core is Markdown, no runtime. The Orchestrator gathers input (the PO agent can't run `gh`/`git`), the PO proposes, the skill files.
- **Decision:** New skill `/backlog` mirroring `/next-steps`' brief-gathering, with a new `## Proposing a Backlog (used by /backlog)` section in `agents/product-owner.md`. Filing uses `gh issue create --repo` by the skill, after confirmation. Issue body follows a fixed template (Problem / Size / Depends on / Source / `generation: 0`) which `/story-time <n>` imports as the idea.
- **Alternatives:** | Extend `/next-steps` | rejected by user: breaks its "never writes" rule |  | Write `.spark/` specs | too heavy, belongs to `/story-time` |
- **Consequences:** One more ceremony; checks (source refs) are best-effort prompt material.

## 3. Task Breakdown
| # | Task | Covers | Definition of done | Status |
|---|---|---|---|---|
| T1 | Add `skills/backlog/SKILL.md` | US-1..5 | steps: gather, delegate, edit loop, confirm, file, report; rules; handoff | done |
| T2 | Add propose-backlog section to `agents/product-owner.md` | US-2 | item fields, source rule, no invention | done |
| T3 | Docs: README table, `docs/workflow.md`, `/spark` and `/next-steps` empty-state pointer, ROADMAP | NFR-2 | mentions consistent | done |
| T4 | Verify: grep consistency, dry-read vs each AC | all | AC table in evidence | done |

## 4. Test Strategy
No runtime; verify by reading each AC against SKILL.md text, and `gh issue create` argument shape by inspection. No live issue filing (would write to the real tracker).

## 5. Risks
Agent may skip source refs (prompt material, best-effort, disclosed in the skill). Partial filing failure: skill reports created issues.

## PLAN GATE
- [x] Every AC covered by a task
