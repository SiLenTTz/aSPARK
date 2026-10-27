# Evidence: adversarial-gates-standard

Prompt material only: the standard makes a strict verdict likely, not certain. **Effect is unmeasured; no rate was observed.**

## T10 Negative baseline (before any edit)
`grep -rn "anti-generosity\|standards/" skills agents templates lenses` → empty; `ls standards` → no such directory. So a repo or run without the standard is unchanged: nothing referenced it. Also true after the edit for any consumer on the old version, by construction (the skills of that version do not pass it). This is a structural dry run, not a session.

## T13 Dispatch and activation, per claimed phase
| | review (`/peer-review`) | qa (`/demo-day`) |
|---|---|---|
| Dispatch | `skills/peer-review/SKILL.md:56` literal path, no constitution read, no flag | `skills/demo-day/SKILL.md:89` same |
| Activation | none: `grep -rn "standards/" skills agents templates lenses` hits only these two lines; `/charter`, `agents/facilitator.md`, `templates/constitution.md`, `lenses/README.md` neither declare nor gate it | same |
| Report names it | present-step line: "If the report does not name the anti-generosity standard as applied, tell the user" (`peer-review:63-64`, `demo-day:101-102`) | |
| Live run | `⚠ not-verified-live` | `⚠ not-verified-live` |

## T14 Verdict-behaviour dry runs
`⚠ not-verified-live`. `claude` is not installed in the build environment (`which claude` → not found), so no real `/peer-review` or `/demo-day` session was run, no planted fixture was graded, and no rate (banned phrases, exact `⚠ not-verified-live` cell, evaluation mode, finding kept) exists. Reasoning about what the file would make an agent do is not a performed step (constitution §8). To be run by a person with the CLI: k=3 per skill on a fixture with one partial check, one static-only check, one real finding and a "good effort" temptation.

## Structural audits
- `wc -l standards/anti-generosity.md` = 40 (AC-2.5, ≤~40).
- `git diff --numstat -- skills`: 5 added / 1 removed in each of the two skills (cap 12; Inc 1 added none) (NFR-1). Only those two skills changed; no frontmatter, command name or ID changed (NFR-3, NFR-7).
- `git diff --stat -- agents templates lenses .spark/constitution.md` empty (AC-2.5, AC-2.7).
- `git ls-files '*.py'` empty; no executable code added (NFR-6). `claude plugin validate .`: `⚠ not-verified-live` (no CLI).
- AC-2.9: rule 1 cites E3, E4 (`.spark/adversarial-gates/evidence.md`); the banned phrases are labelled "Not grounded in the corpus".
- NFR-4: minor bump 0.14.3 → 0.15.0 in `.claude-plugin/plugin.json`; README states the always-on change for installed projects (AC-4.6).
- Silence: no other skill touched; `/go-live` unaffected.
