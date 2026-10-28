# Spec: companion-install-offer

| | |
|---|---|
| **Phase** | Specify |
| **Status** | `approved` |
| **Date** | 2026-10-28 |
| **Ticket** | #45 |

Source: issue #45 (autonomous line; decisions below taken from the issue's own leanings).

## Decisions on the open questions
- **D1 Section number.** `§9` is already *Project Context*; the new section is **§10 Companions**.
- **D2 Boundary.** Written in constitution §6 and in `tools/README.md` § The companion offer: onboarding, not a gate.
- **D3 Guard detection.** None. A skill cannot observe installed plugins; the offer names the command and the user's answer is the only record.
- **D4 No constitution.** One clause in the existing `/spark` nudge, at most once per session, no memory.
- **D5 Handshake (#44).** Out of scope; the guard is offered as-is.
- **D6 Update case.** Re-offer only if recorded version < current, recorded < marker "Companion story last changed in", and a companion is not declared installed.

## Acceptance criteria
- AC-1 `/charter` first draft makes one offer in the existing round; answer recorded in §10.
- AC-2 `/spark` on a repo with no `.spark/` makes the offer (clause of the nudge); no gate ceremony changes.
- AC-3 A decline is recorded and not re-asked; update rule per D6.
- AC-4 No install is ever run.
- AC-5 README and `tools/README.md` entries for the guard (already shipped by `companion-offer`; verified present).
- Prompt-material only: the offer is likely, not certain; no rate measured.
