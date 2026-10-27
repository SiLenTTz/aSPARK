# Spec: observability-lens

| | |
|---|---|
| **Phase** | Specify |
| **Status** | `approved` |
| **Date** | 2026-10-27 |
| **Ticket** | #89 (tracked in #15) |

Approval note: given by the issue's own acceptance criteria in a Factory run; there was no interactive gate.

## Problem & goal
No lens owns whether a running service can be diagnosed. Add `lenses/observability.md` as an add-a-file lens (constitution §1: name the profile that activates it and the profiles where it stays silent).

## Acceptance criteria
- AC-1 Lens names `api` as its trigger and `library`, `cli`, `website`, `web-app` as silent; cross-references `security` instead of duplicating logging hygiene.
- AC-2 Dispatch and activation checked for every phase in its frontmatter (`specify`, `review`) across skills, `agents/facilitator.md`, `templates/constitution.md`; gaps fixed or disclosed.
- AC-3 Negative case first, in this repo (`library`): lens does not fire.
- AC-4 Positive case on a sample `api`: lens fires in Review; labelled a dry run; hold rate stated, not a guarantee.
- AC-5 README, ROADMAP, `docs/status.md` maturity labels honest; stale #6 mention in ROADMAP #15 corrected.
- AC-6 Markdown only; semver bump (a new lens is an addition, so a minor bump).

## Prompt-material note (declared at Specify)
AC-1 (silence on non-`api` profiles) and AC-4 (lens fires in Review) can only be made likely by prompt material, never certain. Neither is a guarantee. The measured rate is n=1 (see evidence.md); no rate is claimed.
