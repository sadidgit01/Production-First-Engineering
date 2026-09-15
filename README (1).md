# Production-First Engineering

> **Build software that survives production—not just the demo.**

A practical engineering skill for the **AI era**.

AI can write code. The harder problem is deciding whether that code should be trusted with **real traffic, real users, real failures, and real money**.

This skill teaches AI-assisted engineering through a production-first lens: **scale, reliability, security, cost, fundamentals, and clear engineering judgment.**

---

## The Core Idea

In the AI era, writing every line yourself is no longer the main measure of engineering ability.

The better questions are:

- Can the system survive real traffic?
- Can it survive real attacks?
- Can it meet its reliability targets?
- Can it do all of that without wasting resources?

A hand-written system that crashes is worse than an AI-assisted system that is **reliable, scalable, secure, and economical**.

> **Don't judge software by whether the demo works. Judge it by whether the system deserves to be trusted in production.**

---

## The Four Pillars

Every system should be evaluated through four primary lenses.

### 1. Scale — Holds Under Load

Can the system keep working as traffic grows?

Common considerations:

- Caching
- Database indexing
- Connection pooling
- Queues and background jobs
- Horizontal scaling

**Red flag:** Works for 10 users, falls over at 10,000.

### 2. Reliability — Meets the SLA

Can the system keep working when things fail?

This applies both to:

- Your own API
- External APIs and services your system depends on

Common considerations:

- Timeouts
- Retries with backoff
- Circuit breakers
- Idempotency
- Graceful degradation

**Red flag:** One third-party outage takes the entire product down.

### 3. Security — Holds Under Attack

Can the system survive common security threats?

Common considerations:

- Authentication
- Authorization
- Input validation
- Rate limiting
- SQL injection prevention
- Secrets management
- Dependency security

**Red flag:** Fast and functional, but insecure.

For concrete security enforcement, this skill works alongside **`secure-build`**.

### 4. Cost — Uses Resources Efficiently

Performance is incomplete without cost awareness.

Common considerations:

- Query optimization
- Memory management
- CPU profiling
- Caching strategy
- Load testing
- Cost monitoring

**Red flag:** Bigger servers are being used to hide inefficient code.

---

## The Production Bar

When trade-offs appear, use this order:

```text
Scale
  ↓
Reliability
  ↓
Security
  ↓
Cost Efficiency
  ↓
Frictionless Delivery
```

Not every project needs massive infrastructure.

The goal is not to over-engineer.

The goal is to choose the **right engineering solution for the actual problem**.

---

## Fundamentals Still Matter

AI assistance does not remove the need to understand what is happening underneath the abstractions.

For backend engineering, the skill emphasizes:

- JavaScript / TypeScript
- Async & concurrency
- HTTP
- SQL
- Transactions
- Memory & event-loop basics
- Networking fundamentals

If you cannot explain what the AI-generated system is doing at this level, you have a knowledge gap worth closing.

---

## Articulation Is an Engineering Skill

AI amplifies precision.

Compare:

> "Optimize my API."

with:

> "Reduce p95 latency of the order endpoint from 420ms to under 200ms; PostgreSQL reads are the bottleneck and traffic is 20× read-heavy."

The second request contains:

- The problem
- A measurement
- The bottleneck
- Traffic characteristics
- A success target

The skill encourages turning vague engineering requests into **precise, measurable problems** before jumping into implementation.

---

## Domain Knowledge Matters

Framework knowledge is replaceable.

Deep understanding of the problem domain is not.

A hospital system, e-commerce platform, and fintech system have completely different failure modes.

Good engineering therefore asks domain-specific questions such as:

> What happens if two patients are booked into the same appointment slot?

Not just:

> Does the CRUD API work?

---

## AI-Assisted Engineering Roadmap

The skill provides a five-stage progression:

| Stage | Focus |
|---|---|
| **1. Independence** | Build core patterns without relying heavily on AI |
| **2. AI as Reviewer** | Use AI for architecture, edge cases, testing, and security review |
| **3. Performance** | Measure bottlenecks and optimize using evidence |
| **4. Operations** | Add logs, metrics, health checks, rate limits, timeouts, and retries |
| **5. Articulation** | Explain architecture, trade-offs, and decisions clearly |

The goal is not to avoid AI.

The goal is to **use AI without becoming dependent on it**.

---

## What This Skill Reinforces

A strong engineer should be able to:

1. Understand a real-world problem
2. Design a reliable system
3. Communicate the design clearly
4. Measure system behavior
5. Identify bottlenecks
6. Improve the system systematically
7. Use AI effectively

That is the progression:

```text
Coder
  ↓
Software Engineer
  ↓
System Architect
```

---

## When to Use This Skill

Use it when:

- Designing backend systems
- Reviewing architecture
- Building APIs
- Designing databases
- Reviewing AI-generated code
- Improving system performance
- Preparing software for production
- Making infrastructure decisions
- Learning system design
- Preparing for system-design interviews
- Writing technical documentation
- Evaluating engineering trade-offs

---

## Related Skills

### `secure-build`

Concrete security enforcement covering:

- Secrets
- RLS
- Rate limiting
- Error handling
- N+1 queries
- Authentication vs authorization

### How They Work Together

```text
Production-First Engineering
          │
          ├── Scale
          ├── Reliability
          ├── Security
          └── Cost
                 │
                 ↓
            secure-build
                 │
                 └── Concrete security enforcement
```

`production-first-engineering` provides the **engineering judgment layer**.

`secure-build` provides the **concrete security rules**.

---

## Philosophy

> **The best engineer is not the one who writes the most code.**
>
> **It is the one who understands the problem, designs the system, measures it, improves it, and knows when the system is ready to be trusted.**