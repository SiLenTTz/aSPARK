---
name: anti-generosity
phases: [review, qa]
activation: always
---

# Anti-generosity standard (review and QA verdicts)

Always applied in `/peer-review` and `/demo-day`; no setting turns it on or off.
Read it in full before you write any verdict. **Name it in the report** ("Anti-generosity
standard applied"). Prompt material: it makes a strict verdict likely, not certain, and its
effect is unmeasured.

**Your known tendency:** a reviewing model rounds up. It calls a partial check clean, lets a
caveat sit beside a green mark, and softens a finding once it has fixed or understood it.
Grade against that tendency.

1. **Binary.** Every rubric item (AC, NFR, checklist row) is PASS or FAIL, with the
   observable that decided it written next to it (command output, `file:line`, observed UI
   state). "Mostly", "partially", "largely", "effectively" are not verdicts.
   *From the corpus:* a ✅ on an AC whose own sentence said "inspection-only" (E3,
   `.spark/adversarial-gates/evidence.md`); `confirmed (performed)` on output a subagent had
   labelled "SIMULATED" (E4).
2. **A partial check is not a pass.** If you only read it, simulated it, or could not run
   part of it, write the Result cell exactly `⚠ not-verified-live`. It never contains `✅`
   or the word "pass" ("partial pass" is read as a pass by tooling), and it names what was
   missing. Reasoning about what code would do is not a performed step.
3. **Say which mode you had.** State once in the report which evaluation mode was actually
   available (live run, tests, static read only) and, if it was less than the item needs,
   that the result is degraded.
4. **No quiet downgrades.** Do not withdraw, shrink or re-grade a finding you recorded
   unless you write the reason and cite its evidence (`file:line` or output). Effort,
   intent, "easy to fix later" and potential earn no credit.
5. **Banned in verdict text:** "solid foundation", "overall good effort", "minor nitpicks
   only", "looks good overall", "essentially done", "should work". State the verdict and
   the open findings instead. *Not grounded in the corpus:* these five are examples chosen
   from the spec, not observed phrases.
6. **The verdict follows the items.** Any FAIL on a Must item, or any `⚠ not-verified-live`
   on a Must item, means the report is not `passed` unless the user waives it in
   writing, recorded in the report.
