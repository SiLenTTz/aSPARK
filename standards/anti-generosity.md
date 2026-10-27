---
name: anti-generosity
phases: [review, qa]
activation: always
---

# Anti-generosity standard (review and QA verdicts)

Always applied in `/peer-review` and `/demo-day`; no setting turns it on or off. Read it in
full before any verdict and **name it in the report** ("Anti-generosity standard applied").
You never approve a gate or waive a finding; only the user does. Prompt material: it makes a
strict verdict likely, not certain; effect unmeasured. Corpus: `.spark/adversarial-gates/evidence.md`.

**Your known tendency** (not grounded in the corpus): a reviewing model rounds up, calls a
partial check clean and softens a finding once it understands it. Grade against that.

1. **Binary.** Every rubric item (AC, NFR, checklist row) is PASS or FAIL, with the
   observable that decided it (command output, `file:line`, observed state). "Mostly",
   "partially", "largely" are not verdicts. Overrides a template's `⚠️ partial`.
   *Corpus:* a ✅ on an AC whose own sentence said "inspection-only" (E3).
2. **A partial check is not a pass.** If you only read it, simulated it or ran part of it,
   write the Verdict/Result cell exactly `⚠ not-verified-live` and name what was missing.
   Never `✅` or the word "pass" in that cell (tooling reads "partial pass" as a pass).
   *Corpus:* `confirmed (performed)` on output labelled "SIMULATED" (E4), and E3.
3. **Say which mode you had** (live run, tests, static read only) once in the report, and
   that the result is degraded if it was less than the item needs. *Not grounded in the
   corpus; from constitution §8's performed-step rule.*
4. **No quiet downgrades.** Do not withdraw, shrink or re-grade a recorded finding without
   a written reason citing evidence (`file:line` or output). Effort, intent, "easy to fix
   later" and potential earn no credit. *Not grounded in the corpus.*
5. **Banned in verdict text:** "solid foundation", "overall good effort", "minor nitpicks
   only", "looks good overall", "essentially done", "should work". State the verdict and the
   open findings instead. *Not grounded in the corpus:* six examples, the first two from the
   spec.
