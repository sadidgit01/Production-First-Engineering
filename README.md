# Production-First Engineering

> **Build software that survives production, not just the demo.**

A Claude skill that designs, reviews, and explains backend systems against the **production bar**: correctness, security, reliability, scale, and cost. It works out how much is at stake first, so a weekend script doesn't get a Kubernetes lecture and a payments API doesn't get a pass.

AI can write code. The harder question is whether that code deserves real traffic, real users, real failures, and real money. This skill makes Claude answer that question precisely.

---

## What it does

| Mode | Triggered by | You get |
|---|---|---|
| **Design** | "How should I build…", "design a schema for…" | Domain failure modes first, the simplest design that meets your stakes, the riskiest decision, and what was deferred and when to add it |
| **Review** | Sharing code, a schema, a repo, or a diagram | Severity-ranked findings (Blocker / Major / Minor), each citing file:line and evidence, plus a concrete fix |
| **Claim check** | "It's scalable / secure / optimized / production-ready" | The claim turned into a testable statement, and the cheapest way to verify it |
| **Explain** | README, portfolio, interview, pitch | 1-sentence / 30-second / 2-minute explanations backed by mechanisms and real numbers |

## How it decides what matters

**1. Calibrate stakes into a tier**

| Tier | Example | Bar |
|---|---|---|
| T0 Prototype | script, notebook, cron job | Correct, no secrets in code, fails loudly |
| T1 Small / internal | class project, <100 users | + validation, auth, timeouts, logging |
| T2 Production | public app | + full heuristic review |
| T3 Critical | payments, health, PII | + threat model, SLOs, load tests, audit, reconciliation |

If you don't state your load or stakes, it assumes them in one line and carries on.

**2. Apply a priority order**

```text
Correctness & data integrity → Security → Reliability → Scale → Cost → Delivery
```

Corrupted data and breaches hurt at any scale, so they come before throughput.

**3. Run concrete heuristics, not generic advice.** For example:

- Outbound call without a timeout → finding
- List endpoint without pagination → finding
- Check-then-insert without a constraint or lock → finding
- Retried webhook without an idempotency key → finding
- Money as float → finding

**4. Lead with domain failure modes.** Double-booking, duplicate charges, invalid status transitions, oversells, OTP brute force. These are the bugs that generic CRUD thinking misses.

## Behaviour guarantees

- **Proportional**: advice depth matches the tier
- **Evidence-based**: every finding cites what it saw; metrics are never invented
- **Doesn't block work**: writes the code you asked for, appropriate to the tier
- **No lectures**: the principles shape its judgment; they aren't recited back

---

## Install

Copy `SKILL.md` into your skills directory (e.g. `~/.claude/skills/production-first-engineering/SKILL.md`), or upload it as a skill in Claude.

### Companion skill: `engineering-growth-roadmap`

`companion/engineering-growth-roadmap/SKILL.md` is a separate coaching skill for learners progressing **Coder → Developer → Backend Engineer → Software Engineer → System Architect**: a five-stage roadmap, hint-first coaching, and a fundamentals checklist. Install it if you want Claude to teach as well as ship.

### Optional: `secure-build`

If you also use a dedicated security skill such as `secure-build`, this skill defers detailed security rules to it. It works fully on its own without one.

---

> **The best engineer isn't the one who writes the most code. It's the one who understands the problem, designs the system, measures it, and knows when it's ready to be trusted.**
