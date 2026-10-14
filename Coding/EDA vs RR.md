# EDA vs Request/Response

Source: [Event-Driven Architecture (EDA) vs Request/Response (RR) - Confluent](https://www.youtube.com/watch?v=7fkS-18KBlw)

Transcript source: [Lilys transcript page](https://lilys.ai/notes/383754)

## Summary

The video compares **Event-Driven Architecture (EDA)** with **Request/Response (RR)** architecture.

- **EDA**: services communicate by producing and consuming events, often through an event stream such as Kafka.
- **Request/Response**: services communicate directly through APIs such as REST or RPC.

The main difference is coupling. Request-response systems connect services directly. Event-driven systems decouple services by letting producers publish facts and letting consumers react independently.

## 1. Reactivity

### Event-Driven Architecture

EDA is reactive. When something happens, such as an order being placed, the service writes an event to a topic. Other services consume that event and react.

Example flow:

```text
Storefront
  -> writes OrderPlaced event
  -> Orders topic
  -> Fulfillment consumes event
  -> Fulfillment processes shipment
```

### Request/Response

RR requires one service to directly drive the next step. The storefront must call fulfillment, or fulfillment must ask the storefront if there is work to do.

Example flow:

```text
Storefront
  -> calls Fulfillment API
  -> waits for response
```

Guideline:

- Use EDA when other services should react to business events.
- Use RR when the caller needs an immediate answer from a specific service.

## 2. Coupling in Space and Time

EDA decouples services in two ways:

- **Space**: the producer does not need to know which consumers exist.
- **Time**: producer and consumer do not need to be online at the same time.

If fulfillment is offline, it can come back later and continue consuming events from the topic. If the storefront is offline, fulfillment can still process events that already exist.

Request-response is more tightly coupled:

- Services must know each other's API details.
- Services must know each other's runtime location.
- Both services usually need to be online at the same time.
- If one service is down, dependent calls fail or must retry.

Guideline:

- Choose EDA when independent availability and asynchronous processing matter.
- Choose RR when direct coordination and immediate response matter more than decoupling.

## 3. Consistency

EDA often creates **eventual consistency**. A consumer becomes consistent with the producer after it processes the relevant events. The delay depends on event volume and consumer processing speed.

Request-response can make consistency easier when all data stays inside one service because there is only one copy of the data.

Important point:

- Any time data is copied, eventual consistency becomes a concern, even in request-response systems.

Guideline:

- Use RR or a single authoritative service when immediate consistency is required.
- Use EDA when eventual consistency is acceptable and decoupling is valuable.
- Be explicit about which data can lag and how much delay is acceptable.

## 4. Historical State

EDA can preserve a history of state changes. A Kafka topic can keep events for as long as configured, allowing services to read:

- From the beginning.
- From the middle.
- From the latest event.
- Again after a bug, failure, or reprocessing need.

This makes event streams useful for replaying history and building new views of past data.

Request-response systems often overwrite data in place. For example, when an order changes, the old version is replaced by the new version. Historical state can be stored, but it is not the default pattern in many request-response database designs.

Guideline:

- Use EDA when event history, replay, auditing, or rebuilding state matters.
- Use RR when only the current state matters and historical changes are not important.

## 5. Architectural Flexibility

EDA makes it easier to add new services because new consumers can subscribe to existing event streams without forcing existing producers to change.

Example:

- Storefront publishes order events.
- Fulfillment consumes order events.
- Later, inventory can also consume order events.
- Another stream can provide incoming stock events.
- Inventory can combine outgoing order data and incoming stock data to build a full inventory view.

In RR systems, APIs are the main building blocks. A new service may need to call multiple existing services to gather the data it needs. If boundaries do not align well, a simple task can become many chained API calls.

Guideline:

- Use EDA when many consumers need to build their own views from shared business facts.
- Use RR when service boundaries are clear and the needed operation maps cleanly to one API.

## 6. Data Access and Reuse

EDA is strong for data reuse. Once events are defined and flowing, the same streams can power:

- Operational applications.
- Data lakes.
- Data warehouses.
- Analytics.
- Machine learning models.
- Vector databases.
- Generative AI workflows.

Tools like Kafka Connect or native consumers can route the same event streams into different systems.

Request-response is mainly designed for real-time operational calls. If you want to populate a data lake or analytics system from many request-response services, you often need separate ETL jobs.

Guideline:

- Use EDA when business data should be reusable across many systems.
- Use RR when the main need is a direct business function exposed through an API.

## Comparison Table

| Factor | Event-Driven Architecture | Request/Response |
| --- | --- | --- |
| Communication | Events through streams/topics | Direct API calls |
| Timing | Asynchronous | Usually synchronous |
| Coupling | Loosely coupled | Tightly coupled |
| Availability | Producer and consumer can be online at different times | Caller and callee usually both need to be online |
| Consistency | Often eventual consistency | Easier immediate consistency when data stays in one service |
| Historical state | Natural fit through retained event streams | Often overwritten unless history is explicitly modeled |
| Adding new services | New consumers can subscribe to existing streams | New services often need more API integrations |
| Data reuse | Strong fit for analytics, ML, AI, data lakes, and derived views | Often requires separate ETL pipelines |

## When to Use EDA

Use EDA when:

- Services need to react to business events.
- Consumers should be independent from producers.
- Work can happen asynchronously.
- Temporary outages should not stop the whole workflow.
- Event history and replay are valuable.
- Multiple systems need to reuse the same business data.
- You are building data-intensive pipelines or derived views.

## When to Use Request/Response

Use RR when:

- A client needs an immediate answer.
- The workflow is simple and direct.
- Strong consistency is easier with a single owner service.
- The caller must know whether the operation succeeded right now.
- The operation maps cleanly to a specific API.

## Practical Takeaway

EDA is best for loosely coupled, asynchronous, reactive systems where events become reusable building blocks. Request-response is best for direct business functions where a caller needs an immediate result from a known service.

Most real systems use both. The important design question is not "EDA or RR everywhere?" It is: **which parts of the system need direct answers, and which parts should react to durable business events?**
