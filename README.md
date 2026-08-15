# System Design Toolkit

A complete end-to-end system design toolkit for learning, practicing, and preparing for real-world architecture discussions and system design interviews.

This repository is organized around three complementary tracks:

- **Theory**: foundational concepts, architectural principles, trade-offs, and design patterns.
- **Code Snippets**: practical implementation examples for common distributed-system components.
- **Interview Prep**: structured problem breakdowns, design templates, estimation practice, and mock interview material.

The goal is to make system design easier to study, easier to apply, and easier to explain under interview pressure.

## Why This Repository Exists

System design is often taught as a scattered collection of diagrams, buzzwords, and memorized case studies. This toolkit takes a more practical approach:

- Build strong fundamentals before jumping into large-scale examples.
- Separate concepts from implementation details so both are easier to revisit.
- Practice reusable design frameworks instead of memorizing one-off answers.
- Connect theory to code through small, focused examples.
- Prepare for interviews with structured prompts, constraints, trade-offs, and review checklists.

Use this repository as a learning path, a reference guide, and a preparation workspace.

## Repository Structure

```text
.
|-- theory/
|   |-- fundamentals/
|   |-- networking/
|   |-- databases/
|   |-- distributed-systems/
|   |-- scalability/
|   |-- reliability/
|   |-- security/
|   `-- patterns/
|
|-- code-snippets/
|   |-- caching/
|   |-- rate-limiting/
|   |-- load-balancing/
|   |-- queues/
|   |-- databases/
|   |-- consistency/
|   |-- search/
|   |-- observability/
|   `-- api-design/
|
|-- interview-prep/
|   |-- templates/
|   |-- estimation/
|   |-- common-problems/
|   |-- case-studies/
|   |-- trade-off-drills/
|   `-- mock-interviews/
|
|-- diagrams/
|   |-- architecture/
|   |-- sequence/
|   `-- data-flow/
|
|-- resources/
|   |-- glossary.md
|   |-- reading-list.md
|   `-- cheatsheets/
|
`-- README.md
```

The exact folders may evolve, but the main separation should remain: theory, implementation examples, and interview preparation.

## Learning Path

### 1. Foundations

Start here if you are new to system design or want to strengthen your basics.

Recommended topics:

- Client-server architecture
- HTTP, REST, WebSockets, and gRPC
- DNS, CDNs, proxies, and load balancers
- Latency, throughput, availability, and durability
- Vertical vs horizontal scaling
- Stateless vs stateful services
- CAP theorem and PACELC
- Consistency models
- Database indexing and query patterns
- Caching strategies
- Message queues and event-driven systems

### 2. Core Building Blocks

Study the reusable components that appear in most large-scale systems.

Key areas:

- API gateways
- Load balancers
- Reverse proxies
- Caches
- Relational databases
- NoSQL databases
- Search indexes
- Object storage
- Message brokers
- Stream processing
- Distributed locks
- Rate limiters
- Background workers
- Monitoring and alerting

### 3. Design Patterns

Learn how components combine into resilient architectures.

Important patterns:

- Read-through cache
- Write-through cache
- Write-behind cache
- Cache-aside
- Sharding
- Replication
- Leader-follower architecture
- Event sourcing
- CQRS
- Saga pattern
- Circuit breaker
- Bulkhead isolation
- Retry with exponential backoff
- Idempotency keys
- Outbox pattern

### 4. Code Snippets

Use the snippets to connect architectural ideas to implementation details.

Examples may include:

- Token bucket rate limiter
- Sliding window rate limiter
- LRU cache
- Consistent hashing
- Pagination strategies
- Retry logic
- Circuit breaker implementation
- Idempotent API handler
- Queue consumer
- Database connection pooling
- Basic pub/sub example
- Health check endpoint
- Structured logging

Code snippets should be small, focused, and easy to adapt.

### 5. Interview Practice

Once the fundamentals are familiar, move into interview-style design problems.

Practice areas:

- Requirement clarification
- Back-of-the-envelope estimation
- API design
- Data model design
- High-level architecture
- Component deep dives
- Bottleneck analysis
- Scaling strategy
- Failure handling
- Trade-off discussion
- Final design summary

## System Design Interview Framework

Use this repeatable structure for most interview problems.

### 1. Clarify Requirements

Identify what the system must do and what is out of scope.

Ask about:

- Core features
- Users and actors
- Read/write behavior
- Scale expectations
- Latency requirements
- Availability expectations
- Consistency requirements
- Security and privacy needs
- Platform constraints

### 2. Define Functional Requirements

Functional requirements describe user-visible behavior.

Example:

- Users can upload media.
- Users can follow other users.
- Users can search public posts.
- The system sends notifications for relevant activity.

### 3. Define Non-Functional Requirements

Non-functional requirements describe system qualities.

Example:

- Highly available
- Low latency reads
- Durable writes
- Horizontally scalable
- Eventually consistent feeds
- Secure by default
- Observable and debuggable

### 4. Estimate Scale

Estimate enough to guide architecture choices.

Common estimates:

- Daily active users
- Requests per second
- Read/write ratio
- Storage per day
- Bandwidth requirements
- Cache size
- Number of database rows
- Queue throughput

### 5. Design APIs

Define the main interfaces before designing internals.

Example:

```http
POST /v1/posts
GET /v1/posts/{post_id}
GET /v1/users/{user_id}/feed
POST /v1/posts/{post_id}/likes
DELETE /v1/posts/{post_id}/likes
```

Good APIs should be:

- Easy to understand
- Versioned
- Idempotent where appropriate
- Authenticated
- Rate limited
- Observable

### 6. Model Data

Choose data models based on access patterns.

Consider:

- Entities
- Relationships
- Primary keys
- Secondary indexes
- Query patterns
- Partition keys
- Retention policies
- Consistency needs

### 7. Build the High-Level Design

Start simple, then scale the design.

Common components:

- Clients
- API gateway
- Application services
- Cache
- Database
- Search index
- Object storage
- Message queue
- Workers
- Notification service
- Analytics pipeline
- Monitoring system

### 8. Deep Dive Into Bottlenecks

Pick the most important parts of the system and explain them well.

Examples:

- Feed generation
- Hot key handling
- Search indexing
- Cache invalidation
- Database sharding
- Media processing
- Ordering guarantees
- Exactly-once vs at-least-once processing
- Data consistency

### 9. Discuss Trade-Offs

Every strong system design answer includes trade-offs.

Examples:

- SQL vs NoSQL
- Strong consistency vs eventual consistency
- Push vs pull architecture
- Synchronous vs asynchronous processing
- Normalization vs denormalization
- Monolith vs microservices
- Batch vs streaming
- Cost vs performance

### 10. Cover Reliability and Operations

Mention how the system behaves in production.

Include:

- Health checks
- Metrics
- Logging
- Tracing
- Alerts
- Retries
- Timeouts
- Circuit breakers
- Backpressure
- Disaster recovery
- Data backups
- Rollback strategy

## Common System Design Problems

This repository can be used to prepare designs for problems such as:

- Design a URL shortener
- Design a rate limiter
- Design a social media feed
- Design a chat application
- Design a notification system
- Design a file storage service
- Design a video streaming platform
- Design a search engine
- Design a ride-sharing service
- Design an online marketplace
- Design a payment system
- Design a collaborative document editor
- Design a metrics and logging platform
- Design a distributed cache
- Design a recommendation system

Each problem should include:

- Requirements
- Assumptions
- Scale estimates
- API design
- Data model
- High-level architecture
- Detailed component design
- Failure scenarios
- Trade-offs
- Interview summary

## Code Snippet Guidelines

Code snippets in this repository should be:

- Minimal but realistic
- Easy to run locally
- Focused on one concept
- Documented with a short explanation
- Accompanied by tests when useful
- Free from unnecessary framework complexity

Each snippet should answer:

- What problem does this solve?
- When should this be used?
- What are the trade-offs?
- What are the failure modes?
- How can it be extended for production?

Suggested snippet format:

```text
code-snippets/<topic>/<snippet-name>/
|-- README.md
|-- src/
|-- tests/
`-- examples/
```

## Theory Note Guidelines

Theory notes should be clear, practical, and interview-aware.

Each concept page should include:

- Definition
- Why it matters
- Common use cases
- Trade-offs
- Failure modes
- Related concepts
- Interview talking points
- Simple diagram when helpful

Suggested concept format:

```markdown
# Concept Name

## Summary

## Why It Matters

## How It Works

## Trade-Offs

## Failure Modes

## Interview Notes

## Related Topics
```

## Interview Prep Guidelines

Interview materials should help convert knowledge into clear communication.

Each interview problem should include:

- Problem statement
- Clarifying questions
- Functional requirements
- Non-functional requirements
- Capacity estimates
- API design
- Data model
- Architecture diagram
- Deep dives
- Bottlenecks
- Trade-offs
- Final answer script
- Follow-up questions

Suggested problem format:

```markdown
# Design <System>

## Problem Statement

## Clarifying Questions

## Requirements

## Estimation

## API Design

## Data Model

## High-Level Architecture

## Deep Dives

## Reliability

## Security

## Trade-Offs

## Final Summary
```

## Design Principles

The toolkit follows these principles:

- **Start simple**: Begin with a clear baseline architecture before scaling.
- **Design from requirements**: Avoid adding components without a reason.
- **Prefer explicit trade-offs**: Name what improves and what gets worse.
- **Know the access patterns**: Data design follows query behavior.
- **Plan for failure**: Distributed systems fail in partial and surprising ways.
- **Make operations visible**: Monitoring, logging, and tracing are part of the design.
- **Separate hot and cold paths**: Optimize frequently used flows independently.
- **Use async processing intentionally**: Queues help decouple systems but add complexity.
- **Cache carefully**: Caches improve latency but introduce invalidation and consistency challenges.
- **Communicate clearly**: A good design is only useful if it can be explained.

## Core Topics Checklist

Use this checklist to track system design readiness.

- [x] Networking basics
- [x] HTTP and API design
- [x] Load balancing
- [x] Performance and reliability basics
- [x] Scaling basics
- [x] Stateless vs stateful services
- [ ] Caching
- [ ] Database indexing
- [ ] SQL vs NoSQL trade-offs
- [ ] Replication
- [ ] Sharding and partitioning
- [ ] Consistency models
- [x] CAP theorem
- [ ] Message queues
- [ ] Stream processing
- [ ] Distributed transactions
- [ ] Rate limiting
- [ ] Search systems
- [ ] Object storage
- [ ] Authentication and authorization
- [ ] Observability
- [ ] Reliability patterns
- [ ] Capacity estimation
- [ ] Common interview problems

## Recommended Study Workflow

1. Read one theory topic.
2. Summarize the topic in your own words.
3. Review or implement a related code snippet.
4. Apply the concept to one interview problem.
5. Write down trade-offs and failure cases.
6. Repeat the design verbally in 5 to 10 minutes.

This mirrors how system design is used in practice: understand the concept, apply it, explain it, and defend the trade-offs.

## Contribution Guidelines

Contributions should keep the repository structured, practical, and easy to navigate.

When adding theory:

- Keep explanations concise but complete.
- Include trade-offs and failure modes.
- Add diagrams when they clarify the idea.
- Link related topics.

When adding code:

- Keep examples focused.
- Include setup and run instructions.
- Avoid unnecessary dependencies.
- Add tests for behavior that is easy to validate.

When adding interview prep:

- Use a consistent problem template.
- Separate requirements from design decisions.
- Include scale estimates.
- Discuss bottlenecks and trade-offs.
- End with a concise interview-style summary.

## Naming Conventions

Use lowercase directory names with hyphens:

```text
distributed-cache/
rate-limiter/
url-shortener/
news-feed/
object-storage/
```

Use descriptive Markdown filenames:

```text
cache-aside.md
consistent-hashing.md
database-sharding.md
feed-generation.md
```

## Diagram Guidelines

Diagrams should be simple and useful. Prefer clarity over visual complexity.

Good diagrams show:

- Request flow
- Data flow
- Component boundaries
- Synchronous vs asynchronous paths
- Storage systems
- Failure boundaries
- Scaling points

Suggested diagram types:

- Architecture diagrams
- Sequence diagrams
- Data flow diagrams
- Deployment diagrams
- State transition diagrams

## Glossary

Common terms used throughout this repository:

- **Availability**: The ability of a system to remain operational.
- **Consistency**: The guarantee that reads reflect expected writes.
- **Durability**: The guarantee that committed data is not lost.
- **Latency**: Time taken to complete a request.
- **Throughput**: Number of operations handled per unit of time.
- **Partitioning**: Splitting data across multiple nodes.
- **Replication**: Copying data across nodes for reliability or performance.
- **Idempotency**: The property that repeated requests have the same effect as one request.
- **Backpressure**: A mechanism for preventing overload by slowing producers.
- **Hot key**: A key that receives disproportionately high traffic.

## Roadmap

Planned repository milestones:

- [ ] Add foundational theory notes
  - [x] Client-server architecture
  - [x] HTTP, REST, WebSockets, and gRPC
  - [x] DNS, CDNs, proxies, and load balancers
  - [x] Latency, throughput, availability, and durability
  - [x] Vertical vs horizontal scaling
  - [x] Stateless vs stateful services
  - [x] CAP theorem and PACELC
- [ ] Add database and caching deep dives
- [ ] Add distributed systems patterns
- [ ] Add runnable code snippets
- [ ] Add common interview problem templates
- [ ] Add complete case studies
- [ ] Add architecture diagrams
- [ ] Add estimation exercises
- [ ] Add mock interview prompts
- [ ] Add review checklists

## Who This Is For

This repository is useful for:

- Software engineers preparing for system design interviews
- Students learning distributed systems
- Backend engineers strengthening architecture fundamentals
- Full-stack engineers moving toward senior-level design discussions
- Anyone who wants reusable notes, snippets, and design templates

## How To Use This Repository

For beginners:

1. Start with `theory/fundamentals/`.
2. Learn one concept at a time.
3. Use the checklist to track progress.
4. Practice small interview problems first.

For interview preparation:

1. Review the interview framework.
2. Practice estimation daily.
3. Solve one common problem at a time.
4. Record trade-offs for each design.
5. Rehearse concise final summaries.

For implementation practice:

1. Pick a building block from `code-snippets/`.
2. Run or implement the example.
3. Add tests.
4. Document production considerations.

## License

Add a license before publishing or accepting external contributions.

Common choices:

- MIT License for permissive open-source usage
- Apache License 2.0 for permissive usage with explicit patent terms
- Creative Commons licenses for documentation-focused repositories

## Maintainer Notes

Keep the repository easy to scan. System design content grows quickly, so structure matters.

When adding new material, prefer:

- One concept per file
- One problem per folder
- Clear headings
- Practical examples
- Explicit trade-offs
- Links between related topics

The best version of this toolkit should help someone move from "I know the terms" to "I can design, explain, and defend a system clearly."
