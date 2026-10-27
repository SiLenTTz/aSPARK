# QA Report: adversarial-gates-standard

| | |
|---|---|
| **Status** | `passed` |
| **Method** | constitution §8 substitute (hands-on against the installed plugin); **no `claude` CLI exists in this environment**, so no ceremony could be invoked. Observed command outputs only. |

- **Verdict:** Structural ACs pass on observed output. Behavioural ACs (AC-2.1–2.4, AC-2.8 live half) and `claude plugin validate` are `⚠ not-verified-live`, all of them Should-level or tooling; no Must AC is unverified. Passed with that disclosure only; accepting the gap is the user's call, not this report's.
- **Anti-generosity standard:** not handed to this run (it is the thing being shipped); applied by hand.

## AC verification (re-run at the QA step, not copied from review)

| Spec ID | Steps performed | Observed | Result |
|---|---|---|---|
| AC-2.5 | `wc -l standards/anti-generosity.md`; `git diff --stat -- agents` | 34 lines; empty diff | ✅ pass |
| AC-2.6 | `grep -rn "standards/" skills agents templates lenses` | only `peer-review/SKILL.md:56`, `demo-day/SKILL.md:89`; no charter/facilitator/template/lens hit | ✅ pass |
| AC-2.7 | `git diff --stat -- templates` | empty | ✅ pass |
| AC-2.9 | read the file | rules 1, 2 cite E3/E4; other rules labelled "not grounded in the corpus" | ✅ pass |
| AC-4.6 | read README "Stricter verdicts" | states always on, changes installed projects, unmeasured | ✅ pass |
| NFR-1 | `git diff --numstat -- skills` | 5/1 per skill (cap 12) | ✅ pass |
| NFR-6 | `git ls-files '*.py'` | empty | ✅ pass |
| AC-2.8 | static text: unconditional literal path; present-step line | text present; no run observed | ⚠ not-verified-live |
| AC-2.1–2.4 | text present in the standard | no graded run | ⚠ not-verified-live |
| NFR-2 live silence re-run | none possible | structural: no other skill changed | ⚠ not-verified-live |
| `claude plugin validate .` | CLI missing | – | ⚠ not-verified-live |

## Bugs
None found.

- [x] Status set to `passed`
