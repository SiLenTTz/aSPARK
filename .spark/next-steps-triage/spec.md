# Spec: next-steps-triage (issue #110)

| | |
|---|---|
| **Phase** | Specify |
| **Status** | `approved` |
| **Ticket** | issue #110 |

## Problem
`/next-steps` ignored the ticket system and proposed features on greenfield projects.

## Change
`skills/next-steps/SKILL.md` and `agents/product-owner.md` now: read tickets (best effort), classify maturity, triage open issues, list gaps with sources, type the recommendation (`existing #N` / `gap` / `foundation`), and emit a Factory JSON block with optional `existing_issue`.

## Acceptance criteria
AC-1..AC-6 as in issue #110. AC-6's Factory side (`proposals.py`) is outside this repo; this repo only defines the field.

## Disclosure
This is prompt material: the checks make the behaviour likely, not certain. Four fixture sessions (1 each) ran on the product-owner agent, all as expected; the no-ticket path and a live run did not. No pass rate is claimed. The unreachable-tickets line follows issue #110 (AC-5) although the constitution §6 says silent fallback; only the user can waive that.
