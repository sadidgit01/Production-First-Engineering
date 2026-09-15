---
name: production-first-engineering
version: 1.0.0
description: Evaluate and build systems against the production bar, not the demo bar - scale, security, SLA reliability, cost efficiency. Activate whenever designing, building, or reviewing systems.
---

# Production-First Engineering in the AI Era

## Core Philosophy

In the AI era, the main question is no longer:

> "Did you write every line of code yourself?"

AI can write a large amount of code. The harder engineering problem is deciding whether that code deserves to be trusted with real users, real traffic, real failures, and real money.

Every system should therefore be judged against the **production bar**, not the demo bar.

Ask:

- Can it survive real traffic?
- Can it survive real attacks?
- Can it meet its required SLA?
- Can it do all of this without wasting unnecessary resources?

A handwritten system that crashes under load is worse than an AI-assisted system that is reliable, scalable, secure, and economical.

> A beautiful demo that cannot be operated in production is a presentation with a build step.

---

# The Production Bar

Evaluate systems through four primary pillars.

## 1. Holds Under Load — Scale

The system should remain usable as traffic and data increase.

Consider:

- Horizontal scaling
- Database indexing
- Caching
- Connection pooling
- Queues and background jobs
- Pagination
- Efficient queries
- Avoiding unnecessary work

Always ask:

> What happens when usage grows from 10 users to 10,000?

A design that works only at demo scale is not production-ready.

---

## 2. Holds Under Attack — Security

The system should remain trustworthy when users, clients, or external actors behave maliciously.

Consider:

- Authentication
- Authorization
- Input validation
- SQL injection prevention
- Rate limiting
- Secure secret management
- Dependency security
- Proper access control

Use the `secure-build` skill alongside this one when the task requires the concrete security and reliability rules defined there.

Security is not a final polishing step. It is part of the architecture.

---

## 3. Meets the SLA — Reliability

The system should continue behaving correctly when components fail.

Consider reliability for both:

1. APIs that your system serves
2. External APIs and services that your system consumes

Use:

- Timeouts
- Retries with backoff
- Circuit breakers
- Idempotency
- Graceful degradation
- Health checks
- Failure isolation

Ask:

> What happens when a dependency is slow or completely down?

A third-party outage should not automatically become a complete product outage.

---

## 4. Uses Resources Efficiently — Cost

A system should not solve performance problems by blindly buying larger servers.

Consider:

- Query optimization
- Database efficiency
- Memory usage
- CPU usage
- Cache effectiveness
- Load testing
- Resource monitoring
- Infrastructure cost

Ask:

> Are we fixing the bottleneck, or simply paying more to hide it?

Cost efficiency is an engineering concern, not only a finance concern.

---

# Priority Order

When making engineering decisions, use this order:

1. Scale
2. Reliability
3. Security
4. Cost efficiency
5. Frictionless delivery

The goal is not to optimize everything prematurely.

The goal is to identify the actual bottleneck, risk, or failure mode and solve it appropriately.

---

# Engineering Fundamentals

AI can generate framework-specific code, but engineers still need to understand the fundamentals beneath the framework.

Core areas include:

- JavaScript / TypeScript
- Async programming and concurrency
- HTTP
- REST APIs
- SQL
- Transactions
- Database fundamentals
- Memory basics
- Event-loop fundamentals
- Networking basics

If a user asks an AI tool to build something they cannot explain at this level, treat that as an **engineering knowledge gap**, not necessarily a blocker to execution.

The system should still be understandable by the person responsible for it.

---

# Make AI-Assisted Work Precise

AI amplifies precision.

Weak request:

> Optimize my API.

Strong request:

> Reduce p95 latency of the order endpoint from 420 ms to under 200 ms. PostgreSQL reads appear to be the bottleneck, and traffic is approximately 20x more read-heavy than write-heavy.

The second request identifies:

- The problem
- The measurement
- The suspected bottleneck
- The traffic pattern
- The success criterion

Before implementation, tighten vague requests into measurable engineering goals whenever possible.

---

# Domain Knowledge Matters

Framework knowledge is replaceable.

Deep understanding of the problem domain is much harder to replace.

When designing a system, ask:

> What can go wrong specifically in this domain?

Examples:

- A hospital appointment system must consider double-booking.
- A payment system must consider duplicate requests and inconsistent payment states.
- A logistics system must consider parcel status transitions, COD reconciliation, rider assignment, and failed deliveries.

Do not stop at generic CRUD thinking.

The correct question is not only:

> "Does the database work?"

It is also:

> "Does the system behave correctly for the real-world problem it represents?"

---

# AI-Assisted Engineering Roadmap

For a backend student progressing toward system architecture, use the following stages.

## Stage 1 — Build Without AI

Learn to independently build:

- REST APIs
- JWT authentication
- Role-based access control
- PostgreSQL schemas
- Transactions
- Error handling
- Pagination
- File uploads

The objective is to establish implementation fundamentals.

---

## Stage 2 — Use AI as a Reviewer

Once the basics are understood, use AI to:

- Review architecture
- Find edge cases
- Suggest tests
- Review security
- Review performance
- Challenge design decisions

AI should become a second engineer, not a replacement for engineering judgment.

---

## Stage 3 — Learn Performance

Study practical bottlenecks such as:

- N+1 queries
- Missing database indexes
- Poor query plans
- Ineffective caching
- Database connection pool limits
- Excessive network calls

Use load-testing tools such as:

- k6
- autocannon

Measure behavior instead of guessing.

---

## Stage 4 — Learn Operations

A production engineer should understand how to observe and operate a system.

Practice:

- Structured logging
- Metrics
- Health checks
- Request IDs
- Rate limiting
- Timeouts
- Retries
- Failure handling

A system is not finished when the code works.

It is finished when the system can also be monitored, diagnosed, and operated.

---

## Stage 5 — Learn to Articulate

Practice explaining a system at three levels:

### 2 minutes

Explain:

- What the system does
- Main components
- Data flow
- Main scaling and reliability decisions

### 30 seconds

Explain:

- The core architecture
- The main bottleneck or risk
- The most important engineering decision

### 1 sentence

Explain:

- What the system is
- Who it serves
- What makes the architecture notable

Good engineers should be able to build the system and explain why it was built that way.

---

# Engineering Mindset

Use this mindset when designing, reviewing, or implementing systems:

1. Understand the real problem.
2. Identify the expected traffic and failure modes.
3. Design for reliability and security.
4. Identify likely bottlenecks.
5. Measure actual behavior.
6. Improve the system systematically.
7. Communicate the trade-offs clearly.
8. Use AI to accelerate execution without surrendering engineering judgment.

The progression is:

> Coder → Software Engineer → System Architect

The goal is not simply to write more code.

The goal is to build systems that deserve to be trusted.

---

# When This Skill Should Activate

Activate this skill whenever the user is:

- Designing a backend system
- Designing an API
- Designing a database
- Reviewing architecture
- Building a production application
- Evaluating scalability
- Investigating performance
- Handling reliability concerns
- Making infrastructure or cost decisions
- Using AI to build backend or system components
- Explaining or documenting a technical project
- Preparing a README, portfolio description, interview explanation, or project pitch

Pay particular attention when the user makes vague claims such as:

- "It is scalable."
- "It is production-ready."
- "It is optimized."
- "It is secure."
- "It can handle a lot of users."

Push toward measurable, bottleneck-aware explanations.

---

# Relationship With Other Skills

## `secure-build`

`secure-build` focuses on concrete security and reliability rules.

`production-first-engineering` focuses on the broader engineering judgment:

- Can the system survive real traffic?
- Can it survive failures?
- Can it survive attacks?
- Can it meet its SLA?
- Is it using resources efficiently?
- Can the engineer explain the decisions?

Use both together when appropriate.

---

# Final Principle

Do not judge software by how impressive it looks in a demo.

Judge it by what happens when:

- traffic increases,
- dependencies fail,
- users behave unexpectedly,
- attackers try to break it,
- data grows,
- latency matters,
- infrastructure costs increase.

Build for the real world.

> Production is the standard.
