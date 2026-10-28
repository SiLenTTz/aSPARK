# Evidence: observability-lens-public-run

| | |
|---|---|
| Feature | observability-lens-public-run |
| Phase | Act / Review (evidence for `review.md`) |
| Date | 2026-10-28 |
| Ticket | #94 |

## 1. Target (AC-1)

| | |
|---|---|
| Repo | `https://github.com/spring-petclinic/spring-petclinic-rest` |
| Change | PR #302 "Fix #287 hide technical exception messages in API error responses" (branch `San-43/fix-287-hide-technical-error-details`), merge commit `c2cdc055385f6e997148fa8dc2d12ec5833b2f38`, merged 2026-03-12 |
| Base (first parent) | `34eff332b11c98d55095a541159c1929a22247d0` |
| Diff | `git diff -M 34eff33 c2cdc05` — 3 files, +48/−2 (`ExceptionControllerAdvice.java`, `OwnerRestController.java`, `OwnerRestControllerTests.java`) |
| Re-run | `git clone https://github.com/spring-petclinic/spring-petclinic-rest && cd spring-petclinic-rest`, read files with `git show c2cdc05:<path>` (do not trust the working tree), then `/peer-review` with `lenses/observability.md` as the only active lens and `api` declared. Every `file:line` below resolves with `git show c2cdc05:<path> \| sed -n '<line>p'` |
| Run date | 2026-10-28; model `claude-sonnet-5-5` for orchestrator and `reviewer` agent |

**Why this is an `api`.** `lenses/observability.md` triggers on the declared type, not on detection: "the type is the trigger". The target meets the lens's own definition, "a backend service that runs somewhere and is operated by someone", on these observable facts at `c2cdc05`: it is a REST service (`@RestController` classes under `rest/controller/`, contract in `src/main/resources/openapi.yml`); it serves HTTP on `server.port=9966` under a context path (`application.properties`); it is deployed as a container (`docker-compose.yml`, `.github/workflows/docker-build.yml`); it has a Spring Boot web starter and an actuator dependency (`pom.xml:58`, `spring-boot-starter-webmvc`). It is not `library`, `cli`, `website` or `web-app` (no UI). Not a project-declared type: the target has no constitution, so the profile `api` was **declared by me in the Reviewer's prompt** (see §2.1).

**Why this diff.** It rewrites an error handler so that a failure's cause is no longer returned to the caller. The lens's central question, "is the failure still recorded server-side?", has something to bite on. Chosen over dependabot bumps (lens silent everywhere) and over PR #329 (already used for the `api` lens run).

## 2. How this run differs from a normal `/peer-review` (AC-3; all `not-verified-live`)

1. **Lens passed by hand; activation chain not exercised.** The target has no `.spark/` trail or constitution. `api` was declared by me and `/workspace/repo/lenses/observability.md` was named in the prompt. The step "read the constitution's active lenses and pass their paths" was not run. This repo's own constitution does not make it an `api`; that is irrelevant to the target.
2. **The trace table was requested.** I asked for one row per checklist item. A default `/peer-review` would not necessarily emit a per-item account. The result therefore measures "the agent can walk the checklist when told to", **not** the unprompted firing rate, which is **not shown**.
3. **Specify half not run.** The target has no spec or NFRs, so "findings tied to `NFR-n`" is untested; every finding is untied.
4. **Not built.** No JVM here. The tests were not run. Anything about runtime behaviour (what Spring logs for a resolved `@ExceptionHandler` exception, which actuator endpoints are exposed, whether `http.server.requests` records the new 404, JPA behaviour of the loaded owner) is `not-verified-live`.
5. **`security` and `api` lenses were off.** The lens says to note "logging content is unchecked: `security` lens off"; the review does. Status-code findings are merits findings, not lens findings.
6. **n = 1.** One run, one model, one reviewer session, no control run without the lens.
7. **Issue #287 not read.** The PR's intended scope is inferred from its title; the network here reaches git remotes only.

## 3. Author's independent pre-read (written before the reviewer was launched)

Scratch file `/tmp/preread.md`, `sha256 a4e90ecb5cd5f41c7a73679ed87b200d4e5ba5e481e288177d96058077c9f118`, hashed before the Reviewer was launched. The hash and ordering are self-reported; the file is not committed (scratch). The Reviewer was given only the diff range, the lens path and the output request. Transcribed:

- P1 `ExceptionControllerAdvice` has no logger; the `DataIntegrityViolationException` handler now discards `ex`. Expected lens 1.1 / 5.1, Major.
- P2 The catch-all `Exception` handler returns 500 with no log record (pre-existing). 5.1.
- P3 The 404 mapping is an `api`-lens status issue, not observability; level misuse (1.3) can't be judged as nothing is logged.
- P4 No request ID anywhere; logback pattern has no MDC field (pre-existing). Minor.
- P5 The new 404 for an unknown owner has no log record; expected client error, so not required at `error` level.
- P6 Actuator is on the classpath, no `management.*`; the diff touches neither health nor metrics → silent.
- P7 No tracing in the project → silent.
- P8 Logs are unstructured (`logback.xml:12`), pre-existing.
- P9 Tests assert the response only, not that anything is logged.

## 4. Per-item record (AC-2)

Vocabulary: **fired** = a finding or a cited clean observation; **silent** = recorded as not applicable to this diff, with reason; **not-verified-live** = depends on a build that did not run. The 12 items are the lens's checklist bullets in order. Verdict column is mine, from primary source at `c2cdc05`.

| Item | Check | Status | Target citation | Author's verdict |
|---|---|---|---|---|
| 1.1 | Failure paths emit a log record | **fired** (F1 Major, F3 Minor); the "Spring only logs at DEBUG" part is not-verified-live | `ExceptionControllerAdvice.java:87-94`; `OwnerRestController.java:137-140`. `git grep -n -iE "logger\|slf4j\|\blog\." c2cdc05 -- src/main/java` → 0 hits | agree; matches P1, P5 |
| 1.2 | Structured logs, timestamp, level | **fired** (F4, pre-existing) | `logback.xml:12` `%-5level %logger{0} - %msg%n` | agree; matches P8. The diff adds no log line, so it is inherited context rather than a defect of the PR |
| 1.3 | Levels used with intent | silent | `ExceptionControllerAdvice.java:87-94` | agree; matches P3 (no log statement exists to judge) |
| 2.1 | Correlation ID in every record | **fired** (cited observation, no finding) | `logback.xml:12`; `git grep -iE "MDC\|correlation\|X-Request-Id" c2cdc05 -- src pom.xml` → 0 hits | agree; matches P4. Reviewer judged the lens's Minor not applicable because no new endpoint was added; I would have filed the Minor. A judgment call, not an error |
| 2.2 | ID propagated and returned | silent | `OwnerRestController.java:136-151` | agree |
| 3.1 | Liveness/readiness reflect dependencies | silent | `pom.xml:58` (actuator present); diff touches no health config | agree; matches P6 |
| 3.2 | Health exposes no internals | silent | `DisableSecurityConfig.java:25` `.anyRequest().permitAll()` | agree |
| 4.1 | New endpoints covered by metrics | silent (whether actuator records the new 404 is not-verified-live) | `OwnerRestController.java:136` (existing endpoint) | agree; matches P6 |
| 4.2 | Bounded labels | silent | none in the diff | agree |
| 5.1 | Unhandled errors reach an error channel | **fired** (F1 Major, F2 Minor) | `ExceptionControllerAdvice.java:87-94`, `:69-75` | agree; matches P1, P2 |
| 5.2 | Critical failures have an alertable signal | silent | none (no spec names a critical failure) | agree. Weak: with no spec the item cannot fire by construction |
| 6.1 | Tracing on new outbound calls | silent | `git grep -iE "tracing\|otel" c2cdc05 -- src pom.xml` → 0 hits | agree; matches P7 |

**Count:** 12 items → 4 fired (1.1, 1.2, 2.1 as a cited observation, 5.1), 8 silent, 0 not-verified-live as a whole item (the label appears inside cells of 1.1, 4.1). Observed once, with the trace prompted (§2.2).

## 5. Findings: right, wrong, missed (AC-2)

| Reviewer finding | Verdict | Basis |
|---|---|---|
| F1 Major (lens): `DataIntegrityViolationException` handler no longer records the cause anywhere | **right** — the static part verified | The handler (`:87-94`) sets a constant detail, the class has no logger (0 `git grep` hits). Whether Spring logs it at DEBUG anyway stays not-verified-live, and root level is INFO (`application.properties:41`). The Reviewer flagged that limit itself. Severity follows the lens's own rule for a swallowed error |
| F2 Minor (lens, pre-existing): catch-all 500 has no `error` record | **right** | `:69-75`; matches P2. Pre-existing, correctly labelled |
| F3 Minor (lens): new 404 path unlogged | **right, low value** | `OwnerRestController.java:137-140`; follows the existing `deleteOwner` pattern at `:126-129`. Reviewer conceded it is an expected client error |
| F4 Minor (lens, pre-existing): no timestamp, no structure | **right, pre-existing** | `logback.xml:12`. Not a defect of the PR |
| F5 Major (merits): `detailBuild` still returns `getLocalizedMessage()` for the 500 and validation handlers | **right** | `:57` read by `:73` and `:112`. A real gap against the PR's own title; whether #287 asked for it is unread. Not a lens finding |
| F6 Minor (merits): title still names the exception class | **right** | `:91`; the new test asserts only `$.detail` (`OwnerRestControllerTests.java:391`) |
| F7 Minor (merits): conflict still mapped to 404 | **right, not a lens item** | `:88`; `api` lens off, so correctly labelled merits |
| F8, F9 Minor (merits): test gaps | **right** | F8 matches P9 |
| F10 Nit: redundant `owner.setId` | **right** | `OwnerRestController.java:143` |

**Missed by the Reviewer.** Nothing from my pre-read. One difference: P4 (missing correlation ID as a Minor) was recorded as a cited observation only. Reviewer's note, not in the pre-read: `addVisitToOwner` (`OwnerRestController.java:172-178`) keeps the pattern the PR removed for pets; outside the diff, so no finding. **Missed by both:** anything that needs a running service.

## 6. Failures and misfires: refuted-with-finding

- **R1 — Firing is prompted, not observed.** The lens docs claim it fires in Review. What this run shows: it fires when the caller names the profile and asks for a per-item trace. Not shown: that a default `/peer-review` with a constitution-activated lens produces the same findings. Route: a follow-up run without the trace request and with a real constitution so dispatch is exercised.
- **R2 — Of 10 findings, 4 are lens findings (F1–F4); 6 (F5–F10) are generic review output** and must not be credited to the lens. Of the 4, 2 (F2, F4) are pre-existing issues outside the diff, correct but not about the PR.
- **R3 — The headline lens finding (F1) rests on a static read.** Its operator impact depends on Spring's runtime logging of resolved exceptions, which was not observed. The lens produced a plausible, citable Major; it did not verify it. The `ROADMAP` criterion "Review-verified on a real `api`" is therefore met **in part**: the lens text ran on a real API diff; nothing was verified against a running service.
- **R4 — Items 3, 4, 6 were silent by design of the diff**, and 5.2 cannot fire without a spec. 8 of 12 silent is the expected result for a 3-file diff, not evidence the checklist is weak or strong. This run says nothing about how those items perform when a diff does touch health, metrics or tracing.
- **R5 — The Reviewer's own note on T1:** it could not see my pre-read, which was by design; the ordering claim (hash before launch) is self-reported and not tamper-evident.
- **R6 — Plan bookkeeping:** `plan.md` marked T1 `done` at creation, which was true (hash taken before launch); T2–T4 were created `pending` and set to `done` only after their artifacts existed.
- **R7 — A correction to the pre-read:** P2 cited the catch-all handler "~:63"; the true range is `:69-75`. The Reviewer's citation is right.

## 7. What this does and does not show (AC-3)

- Lens firing is **instruction-driven**; no test enforces it. 4 of 12 fired, 8 silent in one prompted session: one observation, not a rate, not a guarantee. A second session could differ.
- Shows: with the lens in context and a trace requested, the Reviewer produced on-topic, citable, mostly-correct observability findings on a real public diff, including one (F1: "the fix hides the error from the caller and from the log") that is exactly the lens's stated concern, and it declined to claim runtime facts.
- Does not show: that the lens beats a generic review (no control run), the activation chain, the Specify NFRs, correctness at runtime, or behaviour on a diff that touches health/metrics/tracing.

## 8. Scratch and fences (AC-5)

Clone at `/tmp/target/spring-petclinic-rest` and pre-read at `/tmp/preread.md` are untracked scratch; only `git show/diff/grep/sed` were used. No script written or committed; `lenses/` unchanged. Changes in this repo: this folder, `ROADMAP.md`, `docs/status.md`.
