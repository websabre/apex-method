---

id: RF-DDD-004
title: RideFlow Context Map
version: 1.0
status: Approved
owner: Solution Architecture Team
reviewers:

* Enterprise Architecture
* Domain Experts
  last_updated: 2026-06-29

---

# RideFlow Context Map

> *"A Context Map describes how bounded contexts collaborate without losing their autonomy."*

---

# Purpose

The Context Map defines the relationships, communication patterns, and integration strategies between RideFlow's bounded contexts.

While the Domain Model identifies business concepts and the Bounded Context document defines business boundaries, the Context Map explains how those boundaries interact safely and predictably.

It establishes the rules that prevent tight coupling while enabling business collaboration.

---

# Architectural Principles

The RideFlow Context Map follows these principles:

* Every bounded context owns its data.
* No bounded context accesses another context's database directly.
* Communication occurs through explicit APIs or business events.
* Business events are preferred over synchronous dependencies where appropriate.
* Shared domain models are prohibited.
* Each context evolves independently.

---

# Bounded Context Relationships

| Upstream Context    | Downstream Context | Relationship        | Integration Style |
| ------------------- | ------------------ | ------------------- | ----------------- |
| Identity            | Rider              | Customer / Supplier | REST API          |
| Identity            | Driver             | Customer / Supplier | REST API          |
| Rider               | Ride               | Partnership         | REST + Events     |
| Driver              | Ride               | Partnership         | REST + Events     |
| Ride                | Pricing            | Customer / Supplier | Synchronous API   |
| Ride                | Payments           | Customer / Supplier | Event Driven      |
| Ride                | Notifications      | Published Language  | Domain Events     |
| Ride                | Analytics          | Published Language  | Event Stream      |
| Payments            | Analytics          | Published Language  | Event Stream      |
| Subscription        | Driver             | Customer / Supplier | Domain Events     |
| Support             | Ride               | Conformist          | Read API          |
| Support             | Payments           | Conformist          | Read API          |
| Platform Operations | All Contexts       | Open Host Service   | Platform APIs     |

---

# Relationship Patterns

## Partnership

Ride and Driver collaborate closely.

Both teams agree on business contracts and evolve together.

Examples:

* Driver Assignment
* Ride Acceptance
* Ride Completion

---

## Customer / Supplier

The downstream context depends on services provided by the upstream context.

Examples:

Ride → Pricing

Ride requests fare calculation.

Pricing owns the pricing rules.

---

Subscription → Driver

Driver depends on subscription status.

Subscription owns billing rules.

---

## Published Language

Ride publishes business events.

Consumers subscribe without direct dependencies.

Examples:

RideCompleted

Consumed by:

* Payments
* Notifications
* Analytics

RideCancelled

Consumed by:

* Notifications
* Analytics

---

## Conformist

Support depends on Ride and Payments.

Support does not redefine the business model.

Instead, it consumes their published interfaces.

---

## Open Host Service

Platform Operations exposes reusable platform capabilities.

Examples:

* Feature Flags
* Configuration
* Health APIs
* Audit APIs

Every context may consume these platform services.

---

# Context Communication Matrix

| Context             | Primary Communication |
| ------------------- | --------------------- |
| Identity            | REST API              |
| Rider               | REST + Events         |
| Driver              | REST + Events         |
| Ride                | REST + Domain Events  |
| Pricing             | Synchronous API       |
| Payments            | Events                |
| Subscription        | Events                |
| Notifications       | Events                |
| Analytics           | Event Stream          |
| Support             | Read APIs             |
| Platform Operations | Platform APIs         |

---

# Event Flow

Example: Rider Books a Ride

1. Rider requests a ride.
2. Ride Context validates the request.
3. Ride Context requests fare calculation from Pricing.
4. Pricing returns calculated fare.
5. Ride creates RideRequested event.
6. Driver Context identifies eligible drivers.
7. Driver accepts the ride.
8. Ride publishes RideAccepted.
9. Notifications inform rider and driver.
10. Analytics records operational metrics.
11. Ride completes.
12. Payments processes settlement.
13. PaymentCaptured event updates Analytics.

This sequence demonstrates how business behavior flows across contexts while preserving clear ownership.

---

# Data Ownership

| Context             | Owns Data              |
| ------------------- | ---------------------- |
| Identity            | Users, Roles, Sessions |
| Rider               | Rider Profiles         |
| Driver              | Drivers, Vehicles      |
| Ride                | Ride Lifecycle         |
| Pricing             | Pricing Rules          |
| Payments            | Transactions           |
| Subscription        | Subscription Plans     |
| Notifications       | Notification History   |
| Support             | Support Tickets        |
| Analytics           | Reporting Models       |
| Platform Operations | Configuration & Audit  |

No context may directly modify another context's data.

---

# Integration Patterns

## Synchronous

Used when immediate business responses are required.

Examples:

* Authentication
* Fare Calculation
* Profile Retrieval

---

## Asynchronous

Used when eventual consistency is acceptable.

Examples:

* Notifications
* Analytics
* Payment Settlement
* Subscription Renewal

---

# Failure Strategy

RideFlow assumes that distributed communication may fail.

Guiding principles:

* Retry transient failures.
* Use idempotent event processing.
* Apply Circuit Breaker patterns for unstable dependencies.
* Use Dead Letter Queues for failed event delivery.
* Log and trace every cross-context interaction.

---

# Future Evolution

## Startup

All bounded contexts are implemented as modules inside a Modular Monolith.

Communication is primarily in-process through application services and domain events.

---

## Growth

Introduce asynchronous messaging between selected contexts while remaining within the Modular Monolith.

---

## Scale

Extract bounded contexts into independently deployable services only when justified by:

* Independent scaling requirements
* Independent deployment cadence
* Team autonomy
* Operational complexity
* Business growth

Technology follows business evolution—not architectural trends.

---

# Context Dependency Diagram

```text
                     +----------------------+
                     | Platform Operations  |
                     +----------+-----------+
                                |
                                |
        +-----------------------+-----------------------+
        |                                               |
+-------v-------+                               +-------v-------+
|   Identity    |                               |   Analytics   |
+-------+-------+                               +-------^-------+
        |                                               |
        |                                               |
+-------v-------+                               +-------+-------+
|    Rider      +----------+           +--------> Notifications |
+-------+-------+          |           |        +---------------+
        |                  |           |
        |                  v           |
+-------v-------+    +------+-------+  |
|    Driver     +--->|     Ride     +--+
+-------+-------+    +------+-------+
        |                  |
        |                  |
+-------v-------+    +------+-------+
| Subscription  |    |   Pricing    |
+---------------+    +------+-------+
                           |
                           |
                    +------v-------+
                    |   Payments   |
                    +--------------+
```

---

# Architect's Lens

Questions

1. Which relationships should remain synchronous?

2. Which integrations should evolve into event-driven communication?

3. Which contexts have the highest change frequency?

4. Which contexts are candidates for future microservices?

5. Which integration introduces the highest business risk?

---

# Key Decisions

1. Business ownership determines context ownership.
2. Events are preferred for loose coupling.
3. APIs are used only where immediate responses are required.
4. Every context owns its own data.
5. Shared databases are prohibited.
6. Modular Monolith is the implementation strategy for Version 1.0.
7. The Context Map remains stable even as deployment architecture evolves.

---

# Traceability

Business Capability
→ Domain Model
→ Bounded Context
→ Context Relationship
→ Integration Pattern
→ API / Event
→ Deployment Architecture

This traceability ensures every integration decision has a clear business justification.

---

# Conclusion

The RideFlow Context Map defines how independent business capabilities collaborate to deliver a unified mobility platform.

It provides the blueprint for integration, team ownership, event-driven communication, future microservices, and long-term architectural evolution while preserving the integrity of the domain model.
