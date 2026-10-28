# Review Report: observability-lens-public-run

<!-- aspark-template: aspark/0.1.0 -->

| | |
|---|---|
| **Phase** | Review |
| **Owner** | Reviewer (`/peer-review`) |
| **Input** | Foreign diff `git -C /tmp/target/spring-petclinic-rest diff -M 34eff33 c2cdc05` (spring-petclinic-rest PR #302), `.spark/observability-lens-public-run/plan.md` |
| **Status** | `passed` |
| **Round** | 1 |
| **Date** | 2026-10-28 |

**Handoff**
- **Status:** `passed` means **this review is complete** (the caller's definition for this run). It does not mean PR #302 passes. The gate below shows the target's own state.
- **Verdict:** Changes would be requested on PR #302. The rewritten `DataIntegrityViolationException` handler now records the root cause **nowhere**: it is not in the response and not in any log (F1, lens). The PR also leaves the generic 500 handler leaking `ex.getLocalizedMessage()` (F5, merits).
- **Open:** `10 open`. Blockers: none. Majors: `F1`, `F5`. Minors and Nits: see §3. None can be fixed here because the target clone is read-only.
- **Binding ruling:** §6 Verdict and the gate checklist below.
- **On conflict:** the numbered body below wins for everything except `Status`; log the mismatch as a finding at the next `/peer-review` and proceed — don't stop on it.

## 1. Scope

- **Reviewed:** the 3 files in `34eff33..c2cdc05`: `ExceptionControllerAdvice.java` (+5/-1), `OwnerRestController.java` (+4/-1) and `OwnerRestControllerTests.java` (+39). Every citation is at `c2cdc05` and was resolved with `git show c2cdc05:<path> | nl -ba`. The clone's checked-out HEAD is `afc8fc1`, not `c2cdc05`, so no working-tree file was used.
- **Context read:** `logback.xml`, `application.properties`, `pom.xml` (actuator, parent Boot 4.0.3), `CallMonitoringAspect.java`, `DisableSecurityConfig.java`, and a repo-wide `git grep` for logger, MDC, correlation, tracing and metrics usage.
- **Lens:** `lenses/observability.md`, the only active lens. The profile is `api`, as the caller declared it. **Logging content is unchecked: `security` lens off.** The `api` lens is also off, so F7 (status code) is a review-on-merits finding, not a lens finding.
- **Untied:** the target has no spec, NFRs or constitution, so no finding is tied to an `NFR-n`. Severities follow the lens's phase notes and my own judgment.
- **Not reviewed:** `addVisitToOwner` (`OwnerRestController.java:172-178`) uses the same synthetic-parent pattern that this PR removed for pets. It is outside the diff, so it is noted here and not raised as a finding.
- **Could not check (verbatim):** "Nothing can be built (no JVM)." The test suite was **not run**. Spring's runtime logging of resolved exceptions, JPA behaviour of the now-loaded owner, actuator exposure and Micrometer auto-configuration are all `⚠ not-verified-live`. The text of issue #287 was not read, so the PR's intended scope is inferred from its title.

## 2. Plan Conformance

| Task | Implemented as planned? | Note |
|---|---|---|
| T1 | ⚠️ | Marked `done`. No pre-read file is visible in `.spark/observability-lens-public-run/`. That fits the plan's "Reviewer blind to the pre-read" (it may be held elsewhere), but I could not verify its existence or its sha256 ordering. |
| T2 | ✅ (this report) | This run is T2. Its DoD, "`review.md` written by the agent", is met by this file. |
| T3 | pending | Expected at this point. Not a defect. |
| T4 | pending | Expected at this point. Not a defect. |

## 3. Findings

"Lens" means the observability lens owns the finding. "Merits" means it is a general review finding. Every finding is untied because there is no target spec.

| # | Severity | Location | Finding | Status |
|---|---|---|---|---|
| F1 | Major | `src/main/java/org/springframework/samples/petclinic/rest/advice/ExceptionControllerAdvice.java:87-94` | **Lens 1.1 / 5.1.** The handler swallows the error server-side. The PR replaced `ex.getLocalizedMessage()` with the fixed text "Request could not be processed" (`:92`), and the class has no logger at all (`git grep Logger` over `src/main` finds no matches). The constraint and SQL cause therefore reach no one. Root-level logging is INFO (`application.properties:41`, `logback.xml:19`), and Spring logs resolved `@ExceptionHandler` exceptions only at DEBUG ⚠ not-verified-live. **Why it matters:** an operator who gets a user's "404 Request could not be processed" has no record to diagnose it from. **Fix:** add an SLF4J logger and call `log.warn("Data integrity violation on {} {}", request.getMethod(), request.getRequestURI(), ex)` before line 88. Keep the response generic. | open |
| F2 | Minor | `ExceptionControllerAdvice.java:69-75` | **Lens 5.1, pre-existing.** The catch-all `Exception` handler turns every unhandled fault into a 500 response and writes no `error`-level record, so faults vanish into a generic 500. The PR edited this class without touching it. **Fix:** call `log.error("Unhandled exception on {} {}", …, e)` in this handler. | open |
| F3 | Minor | `src/main/java/org/springframework/samples/petclinic/rest/controller/OwnerRestController.java:137-140` | **Lens 1.1.** The new rejected-request path (unknown owner, returns 404) emits no log record. It follows the existing `deleteOwner` pattern at `:126-129`. The case is an expected client error, which is why this is Minor. **Fix:** add a `debug`/`info` record carrying `ownerId`, or accept the current behaviour explicitly as a project convention. | open |
| F4 | Minor | `src/main/resources/logback.xml:12` | **Lens 1.2, pre-existing, outside the diff.** The pattern `%-5level %logger{0} - %msg%n` has no timestamp and no key/value or JSON structure. Any log line added to fix F1, F2 or F3 would be free text that cannot be queried and carries no time. **Fix:** add `%d{ISO8601}` at minimum, or use Boot's structured logging (`logging.structured.format.console=ecs`) ⚠ not-verified-live for Boot 4.0.3. | open |
| F5 | Major | `ExceptionControllerAdvice.java:57` (used at `:73` and `:112`) | **Merits.** The PR is titled "hide technical exception messages in API error responses", but `detailBuild` still returns `ex.getLocalizedMessage()` for the generic 500 handler and the validation handler. Any untranslated SQL or JDBC error, or any runtime exception message, still reaches callers. The fix covers one exception type out of three handlers. **Fix:** make `detailBuild` use a generic detail for 5xx responses. Keep field-level messages only for 400 validation. Open question: whether issue #287 asked for all handlers. | open |
| F6 | Minor | `ExceptionControllerAdvice.java:91` | **Merits.** `title` is still `ex.getClass().getSimpleName()`, so the response reveals `DataIntegrityViolationException`, a persistence-layer detail. That partly contradicts the PR's intent. **Fix:** use a fixed title such as "Request could not be processed" or `status.getReasonPhrase()`. | open |
| F7 | Minor | `ExceptionControllerAdvice.java:88`; `src/test/java/org/springframework/samples/petclinic/rest/controller/OwnerRestControllerTests.java:391` | **Merits (`api` lens off).** Unknown owners are now caught explicitly (`OwnerRestController.java:137-140`), so any remaining `DataIntegrityViolationException` is a constraint conflict, not a missing resource. The handler still maps it to 404, and the new test pins 404. **Fix:** map it to 409 Conflict (or 400), and assert that status in the test. | open |
| F8 | Minor | `OwnerRestControllerTests.java:378-392` | **Merits, test quality.** The test asserts only `$.detail`. A regression that puts the technical message into `title`, another property or a header would still pass. **Fix:** add `.andExpect(content().string(not(containsString("SQL"))))`, or check against `technicalMessage` (`:385`). | open |
| F9 | Minor | `OwnerRestControllerTests.java:397-408` | **Merits, test quality.** The unknown-owner test checks only the 404 status. It does not prove that the save is skipped. **Fix:** add `verify(this.clinicService, never()).savePet(any())`. | open |
| F10 | Nit | `OwnerRestController.java:143` | **Merits.** `owner.setId(ownerId)` is redundant now that `owner` is loaded by that id (`:137`). It is left over from the removed `new Owner()`. **Fix:** delete the line. | open |

## 4. Requirements Traceability (lens trace table)

There is no target spec, so there are no `AC`/`NFR` rows. Each row below is one lens checklist item, in the order of the lens's bullets. Mark vocabulary: **fired** (a finding or a cited clean observation), **silent** (not applicable to this diff, with the reason), or **not-verified-live**.

| Lens item | Mark | Target citation | Note |
|---|---|---|---|
| 1.1 Failure paths emit a log record | fired | `ExceptionControllerAdvice.java:87-94`; `OwnerRestController.java:137-140` | F1 (Major), F3 (Minor). The Spring-default-logging part of F1 is ⚠ not-verified-live. |
| 1.2 Structured logs, timestamp and level | fired | `logback.xml:12` | F4. Pre-existing. The diff adds no log line, but every fix for 1.1 inherits this format. |
| 1.3 Levels used with intent | silent | `ExceptionControllerAdvice.java:87-94` | The diff adds no log statement, so there is no level to judge. F1 and F3 suggest levels (`warn` and `debug`/`info`). |
| 2.1 Correlation/request ID in every record | fired (cited observation) | `logback.xml:12`; `git grep -i "MDC\|correlation\|X-Request-Id" c2cdc05 -- src` returns nothing | The service has no correlation mechanism. The diff adds no new endpoint (`addPetToOwner` already existed), so the lens's Minor does not apply and no finding is raised. |
| 2.2 ID propagated outbound and returned | silent | `OwnerRestController.java:136-151` | The diff adds no outbound call, and there is no ID to return (see 2.1). |
| 3.1 Liveness/readiness reflect dependencies | silent | `pom.xml:58` (actuator present) | The diff does not touch health. Whether readiness reflects the database is ⚠ not-verified-live, but that is outside this diff. |
| 3.2 Health exposes no internals; auth exemption deliberate | silent | `security/DisableSecurityConfig.java:25` (`permitAll` when security is off) | The diff does not touch health or security configuration. |
| 4.1 New endpoints covered by rate/error/latency metrics | silent | `OwnerRestController.java:136` (existing endpoint); `util/CallMonitoringAspect.java:77` (JMX, repositories only) | The diff adds no new endpoint or job. Whether actuator records `http.server.requests` for the new 404 is ⚠ not-verified-live. |
| 4.2 Bounded metric labels | silent | none in the diff | The diff adds no metric or label. |
| 5.1 Unhandled errors reach an error channel or `error` record | fired | `ExceptionControllerAdvice.java:87-94`, `:69-75` | F1 (the cause is now hidden from both channels) and F2 (the generic 500 handler has no `error` record). |
| 5.2 Critical failures have an alertable signal | silent | none (target has no spec) | No spec names a critical failure. Note that F1 leaves constraint violations with no signal of any kind. |
| 6.1 Tracing on new outbound calls/jobs | silent | `pom.xml` (grep for `tracing\|otel\|opentelemetry\|sleuth` finds nothing) | The project does not trace, and the diff adds no outbound call or job. |

## 5. What Was Checked

- [x] Correctness: the PR hides the message for one handler only (F5). The unknown-owner path was added correctly. JPA behaviour of the loaded, detached owner in `savePet` is ⚠ not-verified-live.
- [x] Non-functional: the observability lens was traced item by item in §4. There are no target NFRs.
- [x] Error handling: F1, F2.
- [ ] Security: not done as a lens (the `security` lens is off). Only the information exposure in error responses was reviewed on merits (F5, F6).
- [ ] Tests: they exist and are meaningful but have gaps (F8, F9). They were **not run** (no JVM) ⚠ not-verified-live.
- [x] Readability: F10.

## 6. Verdict

PR #302 does what its test asserts: a `DataIntegrityViolationException` on `POST /api/owners/{id}/pets` no longer puts the SQL or constraint text into `detail`, and an unknown owner now gets an explicit 404 instead of relying on a foreign-key violation. Seen through the observability lens, though, the change trades a leak for a blind spot. The handler (`ExceptionControllerAdvice.java:87-94`) has no logger, so the cause that used to reach the caller now reaches nobody (F1, Major, matching the lens's "swallowed error" rule). On its merits the fix is also partial: the generic 500 handler still returns raw exception messages (F5, Major), and the response title still names the exception class (F6). With two Majors open I would request changes. All of this rests on reading the code statically. Nothing was built or run, and every runtime claim is marked ⚠ not-verified-live. Lens items 3, 4 and 6 were silent for this diff, as plan §1 predicted. Item 2.1 fired only as a cited observation.

---

## ✅ REVIEW GATE

*This gate describes PR #302's state. `Status: passed` above records that the review is complete, as the caller defined it.*

- [x] No open Blocker findings
- [ ] No open Major findings: F1 and F5 are open, and nobody has waived them
- [ ] Every Must AC traces to implementing code: n/a, the target has no spec. No constitution exists.
- [x] All plan deviations documented and accepted: T1's pre-read could not be seen (§2); T2–T4 are as expected
- [ ] Test suite runs green: not run (no JVM), ⚠ not-verified-live
- [x] Line budget respected: Ist 100 / Soll ~150 (excluding HTML comments)
- [x] Status set to `passed` (meaning the review is complete, not the PR)
