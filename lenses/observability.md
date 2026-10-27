---
name: observability
applies-to: [api]
phases: [specify, review]
---

# Observability Lens

Active when the project profile includes **`api`** — a backend service that runs
somewhere and is operated by someone. A correct, secure, well-specified service can
still be undiagnosable once it is running: the error is swallowed, the request
can't be followed across a hop, nobody is told it is down. This lens checks that
the service **can be diagnosed**. It owns the operability concern between the
`api` lens (the contract) and the `security` lens (what must never be logged).

Like every lens, it flags concerns and proposes NFRs; it never invents scope.

## Where it fires, and where it stays silent

| Profile | Observability lens |
|---|---|
| `api` (a deployed backend service) | **Active** |
| `library` | **Silent** — it has no process of its own to operate; its host owns logging and metrics |
| `cli` | **Silent** — a short-lived process; `cli` owns stdout/stderr and exit codes |
| `website` (static, indexable) | **Silent** — no running service to diagnose |
| `web-app` (SPA, no server of its own) | **Silent** — the `api` it talks to carries the lens |

A project that is both a `web-app` and an `api` (a server-rendered app with its own
backend) is an `api` and gets the lens. There is no separate characteristic: the
type is the trigger, so a service not declared `api` at `/charter` does not get it.

## Boundary with `security` — cross-referenced, not duplicated

This lens asks **"can we see what happened?"**. [`security.md`](security.md) asks
**"did we leak something while recording it?"**. Therefore:

- Secrets, credentials and PII **never** appear in logs, traces or error reports —
  that rule and its severity belong to `security` §PII/privacy and the auth
  lifecycle. This lens does **not** re-check it. If a diff adds a log line, a
  finding about *what it contains* is `security`'s; a finding about *whether it
  exists, is structured and carries a correlation ID* is this lens's.
- When `security` is not active, this lens still does not grow a logging-hygiene
  row; it notes "logging content is unchecked: `security` lens off" in the review.
- The error **envelope** shape returned to callers belongs to `api`; this lens only
  asks that the same failure is also *recorded server-side*.

## Who owns what

| Area | Owner phase | Agent |
|---|---|---|
| Diagnosability intent and measurable signals as NFRs | Specify | Product Owner |
| Logs, health, correlation IDs, error reporting, metrics in the diff | Review | Reviewer |

> **No QA row.** The checks are on server-side code and endpoints, not a browser
> surface — same reasoning as `api`, `cli`, `library`, `data`. **No Plan row:**
> `/sprint-plan` dispatches no lens files; the Specify NFRs carry the intent into
> the plan.

## The checklist

### 1. Logging *(Review)*
- [ ] Every new failure path (caught exception, rejected request, failed downstream call) emits a log record at an appropriate level — no empty `catch`, no swallowed error.
- [ ] Logs are **structured** (key/value or JSON) with a consistent timestamp and level, not free-text concatenation, so they can be queried.
- [ ] Log levels are used with intent: expected client errors are not `error`; unexpected server faults are not `debug`.

### 2. Correlation *(Review)*
- [ ] Each request gets a **correlation/request ID** (accepted from an inbound header or generated), included in every log record for that request.
- [ ] The ID is propagated on outbound calls to other services and returned to the caller (e.g. a response header) so a user report can be traced.

### 3. Health & readiness *(Review)*
- [ ] A **liveness** and a **readiness** signal exist for a deployed service (or the spec states why not), and readiness reflects real dependencies (database, queue), not a constant `200`.
- [ ] The health endpoints expose no secrets or internals beyond up/down per dependency, and are exempt from auth only deliberately.

### 4. Metrics *(Review)*
- [ ] New endpoints or jobs are covered by the service's existing request-rate, error-rate and latency measurement, or the diff adds it.
- [ ] Metric labels are bounded (route template, status class) — no user ID, raw URL or other unbounded value as a label.

### 5. Error reporting & alertable failure *(Review)*
- [ ] Unhandled errors reach the service's error-reporting channel (or at minimum an `error`-level record) and do not vanish into a generic 500.
- [ ] Each failure the spec calls critical has a **signal an alert can key on** (a metric, a log field or a health state) — not just prose in a log message.

### 6. Tracing *(Review, only where the project already traces)*
- [ ] Where the service already uses distributed tracing, new outbound calls and background jobs are inside a span and carry the trace context.

## Phase notes

- **Specify (PO):** turn diagnosability into measurable NFRs — e.g. "every request
  log line carries `request_id`", "`/ready` returns 503 while the database is
  unreachable", "a failed payment capture increments `payment_capture_failed_total`".
  Skip the lens for a feature that touches no running behaviour; say so rather than
  inventing an NFR.
- **Review (Reviewer):** trace the checklist in the diff, each finding tied to its
  `NFR-n`. A swallowed error or an always-`200` readiness probe is a **Major**; a
  missing correlation ID on a new endpoint is a **Minor** unless the NFR makes it
  a requirement. Logging *content* is `security`'s call, not yours.
- **Honesty:** this is an instruction to the agent, not an enforcement; it makes
  the check likely, never certain.
