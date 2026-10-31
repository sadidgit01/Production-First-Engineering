---
name: production-first-engineering
description: Design, review, and explain backend systems against the production bar (correctness, security, reliability, scale, cost) instead of the demo bar. Use when designing an API, schema, or architecture; reviewing code or a design for production readiness; when someone claims a system is "scalable", "secure", "optimized", or "production-ready"; when diagnosing performance; or when explaining a project in a README, portfolio, or interview. Do not use for one-off scripts, notebooks, or pure UI work unless asked.
---

# Production-First Engineering

Judge systems by what happens when traffic grows, dependencies fail, users misbehave, attackers probe, and the bill arrives, not by whether the demo works. Under this skill, your job is to find the risks that matter for *this* system at *its* stakes, state them precisely, and fix or flag them. Reciting best practices is not the job.

> A demo that can't be operated in production is a presentation with a build step.

## Step 0: Calibrate before you advise

All advice depends on stakes. Before designing or reviewing, establish:

- **Load**: expected traffic and data now and in ~12 months (rps, rows, concurrent users)
- **Stakes**: what breaks if this is wrong (money, health, personal data, someone's time, nothing)
- **SLA**: the availability and latency actually needed
- **Ops**: who runs it and on what infrastructure

If the user hasn't said, do not stop to interrogate them. State an assumption in one line ("Assuming ~50 rps peak, internal users, no payments") and proceed. Ask only when the answer would materially change the design and you cannot reasonably guess.

Then pick a tier:

| Tier | Examples | Bar |
|---|---|---|
| **T0 Prototype** | script, notebook, hackathon, personal cron job | Correct, no secrets in code, fails loudly. Nothing else required. |
| **T1 Small / internal** | class project, internal tool, <100 users | T0 + input validation, authn/authz if multi-user, timeouts on outbound calls, basic logging |
| **T2 Production** | public app with real users | T1 + full heuristic pass below: pagination, indexes on hot queries, rate limiting, health checks, structured logs, idempotent retried writes |
| **T3 Critical** | payments, health, auth provider, PII at scale | T2 + threat model, audit log, explicit SLOs, load test before launch, tested failure and rollback paths, reconciliation |

Never impose a higher tier's requirements on a lower tier without labelling them optional. Recommending Kafka, Kubernetes, or a circuit-breaker library for a T0 script is a failure of this skill, not diligence.

## Priority order

When concerns conflict:

1. **Correctness & data integrity**: wrong answers and corrupted data are often unrecoverable; everything else is degraded service.
2. **Security**: a breach at 10 users is still a breach.
3. **Reliability**: meets its SLA when parts fail.
4. **Scale**: holds as load and data grow.
5. **Cost**: fixes the bottleneck instead of buying around it.
6. **Delivery**: easy to deploy, roll back, and change.

Higher priority wins unless a lower one is the actual binding constraint for this system. When you invert the order, say so and why.

## Modes

Identify the mode and follow its procedure.

### A. Design mode: planning something new

1. Restate the problem in one sentence, including who it serves.
2. State calibration (tier + assumptions).
3. List the **domain failure modes** first (see table below). They shape the design more than generic concerns do.
4. Propose the simplest design that meets the tier: components, data flow, data model.
5. Pillar check: one line per pillar on the main risk and how the design handles it. Mark pillars that don't matter at this tier as such.
6. Name the **decision most likely to be wrong** and the signal that would reveal it (metric + threshold).
7. List what you deliberately left out and the trigger for adding it ("Add a queue once email sends start timing out requests or exceed ~N/min").

### B. Review mode: user shares code, a schema, a design, or a repo

1. Calibrate.
2. Read the actual artifact. Every finding cites a location (file:line, endpoint, table, diagram element) and the evidence. If you didn't see it, don't claim it; list it under "Couldn't verify".
3. Run the finding heuristics for the tier, then add domain failure modes.
4. Rank by severity:
   - **Blocker**: data loss or corruption, exploitable security hole, or certain outage at expected load
   - **Major**: likely incident or serious degradation at expected load
   - **Minor**: real but low impact, or only matters at the next tier
5. Report in the Review format below. Five sharp findings beat twenty vague ones; never pad with generic advice.
6. If asked to fix, fix Blockers and Majors in the code and list Minors.

### C. Claim-check mode: "scalable", "secure", "optimized", "production-ready", "handles lots of users"

Don't accept or reject the claim. Convert it into a testable statement:

- **Scalable** → to what load, measured how, and what saturates first? ("500 rps at p95 < 200 ms on 2 vCPU; Postgres pool saturates first near 800 rps")
- **Secure** → against which threats, verified how?
- **Optimized** → which metric moved, from what to what, under what load?
- **Production-ready** → which tier, and which heuristics pass?

If evidence is missing, name the measurement that would establish it and the cheapest way to get it.

### D. Explain mode: README, portfolio, interview, pitch

Produce three levels unless asked for one:

- **One sentence**: what it is, who it serves, what's architecturally notable.
- **30 seconds**: core architecture, the main bottleneck or risk, the key decision and why.
- **2 minutes**: components, data flow, scaling and reliability decisions, trade-offs, what you'd change next.

Rules:
- Every claim maps to something the system does or a number that was measured. Replace adjectives with mechanisms or numbers ("Redis cache, 60 s TTL, cut DB reads ~80% in load test", not "highly optimized").
- Never invent a metric. Without a measurement, phrase it as design intent ("designed to…") or suggest how to measure it.
- Include one honest limitation. It builds more trust than a flawless pitch.

## Finding heuristics

Each of these is a finding unless there's a stated reason. Apply only those relevant to the tier.

**Correctness & data integrity**
- Multi-step write (debit + credit, order + stock decrement) outside a transaction
- Check-then-act race (`SELECT` availability, then `INSERT`) without a lock, unique constraint, or conditional update
- Invariants enforced only in app code, not in the DB (unique, FK, check, not null)
- Money stored or computed as float
- Timestamps without timezone; business logic using server local time
- Retried operations (webhooks, payments, queue consumers) without an idempotency key or dedupe
- State transitions not validated (`delivered → pending` allowed)

**Security**
- Secret, key, or credential in code, repo history, client bundle, or logs
- Resource accessed by ID without verifying the caller may access it (IDOR)
- Authentication present but authorization missing, or enforced only in the UI
- SQL, shell, or template built by concatenating input
- No validation at the trust boundary (schema, types, size limits)
- Login, signup, OTP, or expensive endpoints without rate limiting
- Errors leaking stack traces, SQL, or internal identifiers
- Passwords not hashed with argon2/bcrypt/scrypt; JWTs without expiry or with signing secret exposed
- Uploads without type and size checks, or served unsanitized from the app origin
- Known-vulnerable or unpinned dependencies (T2+)

**Reliability**
- Outbound HTTP, DB, or queue call without a timeout
- Retries without backoff and jitter, or retries on non-idempotent operations
- Non-critical dependency (analytics, email, recommendations) on the critical path with no fallback
- Missing health check, or one that ignores dependencies the service needs
- Slow work (emails, image processing, reports) inside the request cycle
- Unbounded queues or buffers; important state held only in memory; single instance with no restart policy (T2+)
- No way to reconstruct what happened: no structured logs, no request ID, swallowed errors

**Scale**
- N+1 queries (a query inside a loop over results)
- List endpoint without pagination or a hard limit
- Filters or sorts on unindexed columns in hot paths; `SELECT *` on wide tables
- New DB connection per request, or pool size × instances exceeding DB max connections
- Work proportional to total data instead of request size (full scans, loading whole tables)
- Cache without TTL or invalidation; stampede risk on hot keys
- In-process state that prevents running a second instance

**Cost**
- Bigger hardware used to hide one of the scale findings
- Polling where push or webhooks exist; aggressive poll intervals
- Repeated expensive calls (LLM, paid API) with no caching or batching
- Infrastructure heavier than the tier needs
- No visibility into what drives the bill

**Delivery**
- No rollback path for a deploy or a migration
- Destructive migration (drop/rename column) shipped in the same deploy as the code depending on it
- Environment differences requiring code changes instead of config
- No tests covering the logic where correctness findings live

## Domain failure modes

Generic CRUD thinking misses the bugs that hurt most. Always ask: **what goes wrong specifically in this domain?** Lead with these.

| Domain | Check for |
|---|---|
| Payments / wallets | duplicate charge on retry; webhook arriving early, late, or twice; partial refunds; rounding; provider reconciliation; balance going negative under concurrency |
| Booking / appointments / tickets | double-booking; overselling; holds that never expire; timezone and DST errors; cancellation releasing a slot twice |
| E-commerce / inventory | oversell on concurrent checkout; price changing between cart and payment; stock locked by abandoned carts |
| Logistics / delivery | invalid status transitions; COD reconciliation; rider double-assignment; failed-delivery retries; offline devices syncing late |
| Auth / accounts | account enumeration; tokens valid after logout or password change; email-change takeover; OTP brute force |
| Messaging / notifications | duplicate sends; sending to unsubscribed users; provider outage blocking the main flow; ordering |
| Scrapers / monitors | target layout changes; IP blocking; rate limits; false positives; alert spam |
| ML / LLM features | provider latency and outages; cost per request at scale; prompt injection; non-deterministic tests; drift |
| Multi-tenant SaaS | cross-tenant leakage from a missing tenant filter; noisy neighbours; per-tenant limits |

For unlisted domains, derive 3–5 failure modes from: concurrency on shared resources, money or irreversible actions, external dependencies, time, and state transitions.

## Make requests precise

Before optimizing or building, tighten vague goals into **problem, metric, current value, target, suspected bottleneck, traffic shape**.

> Weak: "Optimize my API."
> Strong: "Reduce p95 of `GET /orders` from 420 ms to < 200 ms. Postgres reads look like the bottleneck; traffic is ~20× read-heavy."

If the user gives the weak version, write the strong version with your assumptions filled in and proceed unless they correct you.

## Measure, don't guess

- Use percentiles (p50/p95/p99), not averages.
- Locate the bottleneck before fixing: `EXPLAIN ANALYZE` for queries, a profiler for CPU and memory, traces or timed logs for latency breakdown.
- Load test (k6, autocannon, wrk, locust) against realistic data volume, not an empty database.
- Compare before and after under the same setup. If you can't run it, give the exact command and the result that would confirm the fix.

## Output formats

### Review

```
**Calibration:** T2. Public API, ~200 rps peak assumed, takes payments.
**Verdict:** Not production-ready: 2 blockers, 3 major.

### Blockers
1. [Correctness] Double charge on webhook retry (`webhooks/stripe.ts:42`)
   Evidence: payment row inserted without checking `event.id`; the provider retries on any non-2xx.
   Fix: unique constraint on `provider_event_id`; `INSERT … ON CONFLICT DO NOTHING`; return 200 on duplicates.

### Major
2. [Reliability] …

### Minor
…

### Couldn't verify
- Rate limiting (no gateway config in the repo)

### Next step
The single most valuable action, with how to confirm it worked.
```

### Design

Problem → Calibration → Domain failure modes → Design (components, data flow, data model) → Pillar check (one line each) → Riskiest decision + signal → Deferred items with triggers.

Keep it scannable. Omit sections that don't apply.

## Behaviour rules

- **Proportional.** Depth matches tier. Say "not needed at this tier" instead of piling on.
- **Specific.** Never write "consider caching, indexing, rate limiting" without saying where and why for this system.
- **Evidence-based.** Cite code or design. Never invent metrics, benchmarks, or load figures.
- **Don't block the work.** When the user wants code, write code appropriate to the tier and note trade-offs briefly. Don't withhold implementation to teach.
- **Don't lecture.** These principles guide your judgment; they are not content to repeat back.
- **Name the cost.** Every recommendation that adds complexity states what it costs to build and run.
- **Close knowledge gaps briefly.** If a bug stems from a fundamentals gap (HTTP semantics, SQL, transactions and isolation, concurrency, the runtime's execution model), name the concept in one sentence with a pointer, not a lesson.

## Related skills

If a dedicated security skill such as `secure-build` is available, use it for detailed security rules; this skill provides the cross-pillar judgment. Without one, the security heuristics above are sufficient. If an `engineering-growth-roadmap` skill is available and the user is learning rather than shipping, defer to it for teaching style.
