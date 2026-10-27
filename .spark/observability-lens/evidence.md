# Evidence: observability-lens

All runs are dry runs, not public runs.

## T4 — Negative case, run first (AC-3)
Repo: this one, profile `library` (`.spark/constitution.md` §2). The active-lens table lists only `library`; I added `observability` to the existing "Off" row for `api` (`api`, `observability`), the only change to the constitution. Dispatch is `peer-review/SKILL.md:49-52`: pass only *active* lenses whose `phases` include `review`. `observability` is not active, so no path is passed and nothing in `/peer-review` or `/story-time` changes. Before and after, the lens count is 1, and no other file in a `library` run reads `observability.md`. Status: holds by construction; not run against a live session.

## T5 — Positive case (AC-4)
Sample: `/tmp/sample-api` (Flask, two routes, constitution with `api` and `observability` active). A fresh general-purpose agent acting as the Reviewer, given `agents/reviewer.md`, the profile, `app.py` and the lens. Result: **lens applied (1 of 1 sessions)**. Findings F1 swallowed `except: pass` (Major, §1/§5), F2 constant-`ok` `/health` (Major, §3), F3 no correlation ID (Minor, §2), F4 no alertable signal (Minor, §5). It deferred log content to `security` and noted "logging content is unchecked: `security` lens off". Hold rate: **n=1, so no rate is claimed**; the check is prompt material and makes the finding likely, never certain. Caveat: the agent was handed the lens path directly; whether `/peer-review` passes it is shown by the AC-2 reading, not by this run.

## T7 — Dispatch and activation per claimed phase (AC-2)
Frontmatter claims `specify`, `review`.
| Phase | Dispatch | Activation |
|---|---|---|
| specify | `skills/story-time/SKILL.md:53-70` passes all active lens paths (generic) | `lenses/README.md` registry row + `agents/facilitator.md:71` mapping + `templates/constitution.md:26-28` mapping, all updated |
| review | `skills/peer-review/SKILL.md:49-52` passes by `phases` ∋ `review` (generic, no name list) | same |
Not claimed, checked: `design` (`look-and-feel` reads `phases`; the lens declares none); `qa` (`demo-day` ditto; `agents/qa-tester.md:192` lists non-QA lenses and `observability` was added to it); `plan` (`sprint-plan` and `increment` contain no lens text, so a plan row cannot exist). Disclosed gap: no Plan-phase dispatch exists for any lens; the Specify NFR carries intent into the plan (stated in the lens). The Facilitator reads the registry for characteristics, and this lens adds none.
Result: no closed-enumeration gap found for the claimed phases; the three name-list copies (facilitator, constitution template, `qa-tester.md`) were updated by hand.

## Version
`.claude-plugin/plugin.json` 0.14.3 → 0.15.0 (AC-6; the library lens says additions are minor). `CONTRIBUTING.md` says "don't bump the version"; the issue's AC-6 overrides it for this PR. The maintainer can revert that line.
