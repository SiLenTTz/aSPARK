---
name: open-findings
phases: [review, qa, keep]
activation: always
---

# Open findings → follow-up ticket proposals

Run by the conductor (the main conversation, not a subagent) at the end of
`/peer-review`, `/demo-day` and `/go-live`, after the gate decision. Prompt
material: it makes a complete list likely, not certain; rate unmeasured.

1. **Collect** what stays open past the gate, from `review.md` / `qa.md` (in
   `/go-live`, from both): open Major and Minor findings (not Fixed, not
   Nits the user dropped), checks marked `not-verified-live`, accepted risks
   and waived Majors, follow-ups the report calls out of scope, and changes it
   says belong in another repo. Fixed findings are never collected.
   **None → stop: no section, no question, no output** (AC-5).
2. **Merge** findings with the same cause into one proposal.
3. **Check for an existing ticket** (best effort): reuse the `/next-steps`
   triage, `gh issue list --state open --json number,title,body --limit 200`
   (or `glab`, or a ticket MCP). A finding an open issue already covers is
   listed as `covered by #N` and not proposed again. If no tool works, say so
   in one line and propose without the check.
4. **Build each proposal:** title, one-paragraph body, source
   (`review.md F3`, `qa.md B2`), parent ticket (the spec's `Ticket` row, or
   `none`).
5. **Show the list** (proposals plus `covered by #N` lines).
   - **Interactive:** ask for explicit confirmation to file, naming count and
     repo (resolved from `origin`; echo it; none unambiguous → stop). Only a
     yes in this conversation permits filing (AC-2); no answer or no → file
     nothing. File as `/backlog` step 7 does: `gh issue create --repo
     <owner/repo> --title '<title>' --body-file -`, body from a single-quoted
     heredoc, ending with `generation: <parent generation + 1>` (parent's
     `generation:` line + 1; no parent or no line → `generation: 1`) and a
     `Parent: #<n>` and `Source:` line. Report the issues created, or the real
     `gh` error. Never file silently.
   - **Factory** (`agent.yaml` exists or the caller asks for result JSON, and
     nobody can answer): do not file. Add an optional `followups` array to the
     session's result JSON, only when there is at least one proposal. Shape in
     `docs/workflow.md`.
