# Spec: backlog-dedupe (issue #114)

| | |
|---|---|
| **Status** | `approved` |
| **Date** | 2026-10-28 |

`/backlog` reads open and closed issues and merged PRs before showing its list, marks items `new` / `covered by #N` / `done in #N / PR #M` / `partly covered`, files only `new` by default, and says once when `gh` is unavailable.

ACs: AC-1 open match → `covered by #N`, not filed. AC-2 closed by merged PR → `done`, not filed. AC-3 re-run right after filing files nothing. AC-4 no `gh`: one line, else as before. Out of scope: editing or closing issues.
