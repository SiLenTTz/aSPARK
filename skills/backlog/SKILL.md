---
name: backlog
description: >
  Seed a backlog from the project's documents: the Product Owner reads docs/
  (plus the constitution and ROADMAP.md), proposes an ordered list of
  feature-sized items each traced to a source section, the user edits the list,
  and on explicit confirmation each item is filed as a GitHub issue that
  /story-time <n> can import. Use on a new or document-first repo with no
  backlog, or when a tracker or runner needs proposed work as issues.
---

# /backlog — Seed a Backlog from docs/ (Product Owner)

An **advisory-then-writing** ceremony with one gate. The Product Owner proposes
an ordered backlog from the project's own documents; the user edits it; only on
explicit confirmation are issues created. Nothing is written to `.spark/`, and
no spec is created here: specs belong to `/story-time`.

## Input

Optional: `--label <name>` (repeatable) or a steer such as "only the MVP". No
other input is needed.

## Steps

1. **Gather the sources yourself.** The `product-owner` agent cannot read the
   repo's remote or run `gh`. Read:
   - `docs/` (Markdown/text documents only; read each section's heading and
     lead paragraph, not full bodies, and note file names and headings, as
     source references need them. Skip binary or non-text files and name what
     was skipped in the step 8 report),
   - `.spark/constitution.md` and `ROADMAP.md` if present.
   - **If there is no `docs/` (or it holds no readable documents) and no
     constitution**, name `/charter` as the first step and stop there. Never
     invoke it yourself, and write nothing. Same rule as `/next-steps`.
2. **Delegate to the Product Owner.** Invoke the `product-owner` agent in its
   *propose backlog* mode (`## Proposing a Backlog` in its definition) with the
   document excerpts (file + section headings), the constitution excerpt or
   "none", `ROADMAP.md` if present, and the user's steer.
3. **Show the list as a plan.** Present it as a numbered, ordered table: title,
   problem statement, size (S/M/L), dependencies, source reference
   `docs/<file>#<section>`. Drop any item that arrives without a source
   reference, or whose reference names no file and heading you gathered in
   step 1: nothing is invented. This check is an instruction to you and
   the agent, not an enforcement; look at every row yourself.
4. **Let the user edit.** They may drop, merge, reorder or reword items.
   Apply the edits (re-invoke the agent for merges/rewording if useful) and
   re-show the list. Repeat until the user confirms the final list or
   abandons it. Abandoning writes nothing.
5. **Resolve the repo and labels, once, before filing.**
   - Repo: from `origin` (`git remote get-url origin` / `gh repo view`), as
     in `/story-time`. Echo the resolved `owner/repo` to the user. No
     unambiguous `origin` → **STOP** and say so; never guess.
   - Labels: default **none**. Use labels passed as arguments; otherwise the
     `labels:` list in the optional `.spark/backlog.yml`; arguments win.
     Echo them (or "no labels"). Apply them with `--label`; do not assume any
     particular automation behind them.
6. **Ask for explicit confirmation to file**, naming the count, repo and
   labels. If the repo is public, say so here, once: the content of `docs/`
   goes into the issue bodies. Only a yes in this conversation permits step 7.
7. **File in order.** For each item run
   `gh issue create --repo <owner/repo> --title '<title>' --body-file -`
   (plus `--label` per label), feeding the body on stdin from a
   single-quoted heredoc (`<<'ASPARK_BACKLOG_BODY'`). Never put text from
   `docs/` inside double quotes: the shell would run any backtick or `$(...)`
   in it. If any body line equals the delimiter, pick another one: that line
   would end the heredoc and the shell would run the rest. In the
   single-quoted title, write each `'` as `'\''`. Record each returned issue
   number; in later bodies, refer to dependencies already filed as `#<n>`,
   otherwise by title.
   The body is exactly:

   ```
   ## Problem
   <short problem statement>

   **Size:** <S|M|L>
   **Depends on:** <#n, titles, or none>
   **Source:** <docs/<file>#<section>, or the constitution / ROADMAP.md section>

   generation: 0
   ```

   This is the shape `/story-time <n>` imports as the idea. If `gh` is
   missing, unauthenticated or a call fails, **stop**, report the real error
   and list the issues created so far. Never retry silently or claim success.
   If the user fixes the cause in this conversation, offer to file only the
   items not yet created, re-confirming per step 6, so nothing is duplicated.
8. **Close with a report** stating: repo, issues created (number + title),
   labels applied, items dropped for lack of a source, files in `docs/`
   skipped as non-text in step 1 (or "none"), and that
   `/story-time <n>` or `/spark` starts any of them.

## Rules

- Never create an issue before the explicit confirmation in step 6.
- Never write or modify files under `.spark/`.
- Every item has a `docs/<file>#<section>` source (or the constitution's
  section or ROADMAP entry named as such); otherwise it is not proposed.
- Labels are optional and tracker-agnostic; default is none.
- Every issue carries the `generation: 0` line.

## Handoff

- Issues filed → **`/story-time <n>`** or **`/spark`** per issue, on the user's go.
- User abandons → stop; nothing persists.
