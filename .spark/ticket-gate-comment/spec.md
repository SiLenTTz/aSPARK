# Spec: ticket-gate-comment

| | |
|---|---|
| **Phase** | Specify |
| **Status** | `approved` |
| **Date** | 2026-10-28 |
| **Ticket** | #115 |
| **Gate comment** | none |

## 1. Problem & Goal
Seeded from issue #115. A ticket-seeded `/story-time` that stops at the SPEC GATE asks only in the conversation; the ticket reader sees no question and cannot answer. Goal: post the gate question to the ticket once and accept a write-access answer there.

## 2. Acceptance criteria
- AC-1: a spec from `/story-time 42` stopping at the gate posts exactly one comment on #42 (summary, open questions with recommended options, how to answer).
- AC-2: a rerun with the comment URL recorded in the spec header posts nothing.
- AC-3: a rerun after the owner answered "approve, option B" takes it as the decision, quotes comment and author, records the link.
- AC-4: a comment from someone without write access is not approval.
- AC-5: `Ticket` = `none` unchanged.

## 3. NFR / limits
Core is Markdown prompt material: every AC is an instruction, likely but not certain to be followed; no measured rate yet.

## 4. Out of scope
Factory-side resume of parked issues (aSPARK-Factory).

Approval: autonomous run, recommended option taken at the gate.
