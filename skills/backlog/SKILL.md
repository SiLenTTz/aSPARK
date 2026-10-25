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
   - `docs/` (every Markdown/text document; note file names and section
     headings, as source references need them),
   - `.spark/constitution.md` and `ROADMAP.md` if present.
   - **If there is no `docs/` (or it holds no readable documents) and no
     constitution**, name `/charter` as the first step and stop there. Never
     invoke it yourself, and write nothing. Same rule as `/next-steps`.
2. **Delegate to the Product Owner.** Invoke the `product-owner` agent in its
   *propose backlog* mode (`## Proposing a Backlog` in its definition) with the
   document excerpts (file + section headings), the constitution excerpt or
   "none", and the user's steer.
3. **Show the list as a plan.** Present it as a numbered, ordered table: title,
   problem statement, size (S/M/L), dependencies, source reference
   `docs/<file>#<section>`. Drop any item that arrives without a source
   reference: nothing is invented. This check is an instruction to you and
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
   labels. Only a yes in this conversation permits step 7.
7. **File in order.** For each item run
   `gh issue create --repo <owner/repo> --title "<title>" --body "<body>"`
   (plus `--label` per label). Record each returned issue number; in later
   bodies, refer to dependencies already filed as `#<n>`, otherwise by title.
   The body is exactly:

   ```
   ## Problem
   <short problem statement>

   **Size:** <S|M|L>
   **Depends on:** <#n, titles, or none>
   **Source:** docs/<file>#<section>

   generation: 0
   ```

   This is the shape `/story-time <n>` imports as the idea. If `gh` is
   missing, unauthenticated or a call fails, **stop**, report the real error
   and list the issues created so far. Never retry silently or claim success.
8. **Close with a report** stating: repo, issues created (number + title),
   labels applied, items dropped for lack of a source, and that
   `/story-time <n>` or `/spark` starts any of them. If an issue body is
   public, say so once before filing: the content of `docs/` goes into it.

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
