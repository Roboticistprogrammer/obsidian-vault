# System Design

Source: [900+ hours of Learning System Design in 9 Minutes - Maddy Zhang](https://www.youtube.com/watch?v=3Pusamd6BO4)

Transcript source: [Rewiz transcript/summary](https://rewiz.app/channels/%40maddy-zhang/system-design-was-hard-until-i-learned-these-6-concepts)

## Summary

System design is not about memorizing a glossary of tools like DNS, sharding, CDNs, Redis, Kafka, or GraphQL. The real skill is learning the principles that let you reason through architecture tradeoffs.

The video highlights six concepts that make system design easier to understand:

1. Statelessness.
2. Caching.
3. CAP theorem.
4. Message queues.
5. Databases.
6. API design.

## 1. Statelessness

Stateless servers do not store user-specific request history locally. Each request should carry what the server needs to process it, usually through tokens or shared external state.

Why it matters:

- Stateless servers are interchangeable.
- Load balancers can route requests to any healthy server.
- Servers can be added or removed more easily.
- Failures are less destructive because important state is not trapped on one machine.

Avoid:

- Local session state on one server.
- Sticky sessions unless there is a strong reason.
- Shopping carts, login state, or workflow progress stored only in app-server memory.

Guideline:

- Before asking how to scale a service, ask: **What state is this server holding, and where should that state actually live?**

Common solution:

- Move shared state into Redis, a distributed cache, a database, or another external store.

## 2. Caching

Caching trades freshness for speed. A cache stores a copy of data closer to where it is needed so requests are faster, but the cached data may be slightly stale.

Where caching appears:

- Browser cache.
- CDN.
- Application cache such as Redis.
- Database query cache.

Use cases:

- **CDN**: static assets and content that changes rarely.
- **Redis / app cache**: read-heavy queries that need low latency.
- **Database cache**: repeated database queries.

Important caching concepts:

- **TTL**: how long cached data remains valid.
- **Cache-aside**: app checks cache first, falls back to database, then updates cache.
- **Write-through**: writes update cache and backing store together.
- **Write-back**: writes hit cache first and are persisted later.

Guideline:

- Do not ask "Should I use Redis or a CDN?" first. Ask: **Where is the bottleneck, and how stale can the data be?**

## 3. CAP Theorem

CAP theorem is often summarized as choosing two out of:

- **Consistency**: every read sees the latest write.
- **Availability**: every request receives a response.
- **Partition tolerance**: the system continues despite network failures.

In real distributed systems, network partitions are unavoidable, so partition tolerance is usually a given. The real tradeoff is between consistency and availability during failure.

Guidelines:

- Do not choose consistency or availability globally for the entire system.
- Different operations can need different guarantees.
- Financial transactions, permissions, and access control usually need strong consistency.
- Feeds, recommendations, analytics, and counters can often tolerate eventual consistency.

Interview answer pattern:

- Explain which operations need strong consistency.
- Explain which operations can be eventually consistent.
- Explain why the tradeoff is acceptable for the product.

## 4. Message Queues

Message queues decouple services by moving work into asynchronous events.

Without a queue:

- A user action may call inventory, payment, notification, analytics, and other services synchronously.
- If one downstream service is slow or down, the whole request can fail.

With a queue:

- The main service publishes an event such as `OrderPlaced`.
- Kafka, SQS, or another queue stores the event.
- Downstream services consume and process the event independently.

Benefits:

- Better reliability.
- Services fail independently.
- Slow background work does not block the user.
- Temporary failures can be retried later.
- Traffic spikes can be buffered.

Guideline:

- Use queues when work can happen asynchronously and the user does not need every downstream result immediately.

## 5. Databases

The SQL vs NoSQL choice is not about old vs new. It is about the guarantees and access patterns the system needs.

### SQL

SQL databases are a good fit when the system needs ACID guarantees.

ACID means:

- **Atomicity**: a transaction is all-or-nothing.
- **Consistency**: the database moves from one valid state to another.
- **Isolation**: concurrent transactions do not corrupt each other.
- **Durability**: committed data survives crashes.

Use SQL for:

- Financial transactions.
- Inventory systems.
- Relational data.
- Workflows where partial writes are dangerous.
- Data that needs strong constraints and correctness.

### NoSQL

NoSQL databases trade some strict guarantees for scale, flexibility, and horizontal distribution.

Use NoSQL for:

- Activity feeds.
- Product catalogs.
- Real-time analytics.
- Massive-scale writes.
- Flexible or evolving schemas.
- Use cases where slight staleness is acceptable.

Guideline:

- Ask: **Does this part of the application require ACID guarantees?**
- Many production systems use both SQL and NoSQL for different parts of the workload.

## 6. API Design

An API is a contract with every client that depends on it. Once shipped, changing it becomes a migration problem, not just a code change.

### REST

REST is useful when the system needs:

- Simplicity.
- Cacheability.
- Stable public interfaces.
- Resource-oriented endpoints.
- Easy documentation and debugging.

### GraphQL

GraphQL is useful when the system has:

- Many clients with different data needs.
- Web and mobile apps requiring different response shapes.
- A need to avoid over-fetching or under-fetching.

Guidelines:

- Version APIs from the beginning.
- Design endpoints around resources, not internal implementation details.
- Document contracts before others have to reverse engineer them.
- Treat API changes as downstream coordination work.

## Practical Interview Advice

- Learn principles, not just tool names.
- For every concept, explain the tradeoff it solves.
- Apply each concept to real products you use.
- Practice questions like:
  - How would Spotify use caching?
  - When would Venmo need strong consistency?
  - Which parts of an order flow can be asynchronous?
  - Which API style fits a public API versus a multi-client product?

## Practical Takeaway

Good system design comes from asking the right tradeoff questions:

- What state exists, and where should it live?
- What can be cached, and how stale can it be?
- Which operations need consistency, and which can favor availability?
- What can be handled asynchronously?
- What guarantees does the database need?
- What contract does the API expose to clients?

These questions matter more than memorizing architecture buzzwords.
