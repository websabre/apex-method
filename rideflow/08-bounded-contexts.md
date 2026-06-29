---

id: RF-DDD-003
title: RideFlow Bounded Contexts
version: 1.0
status: Approved
owner: Solution Architecture Team
reviewers:

* Domain Experts
* Enterprise Architecture
  last_updated: 2026-06-29

---

# RideFlow Bounded Contexts

> *"Bounded Contexts define business boundaries, not deployment boundaries."*

---

# Purpose

This document defines the bounded contexts within the RideFlow platform using Domain-Driven Design (DDD).

A bounded context represents a well-defined business boundary within which a particular domain model is valid.

Bounded contexts should **not** be confused with microservices.

A single bounded context may initially be implemented as one or more modules inside a modular monolith and later evolve into one or more independently deployable services if business and operational needs justify such a change.

---

# Design Principles

The RideFlow bounded contexts are designed according to the following principles:

* High cohesion
* Low coupling
* Single business responsibility
* Independent evolution
* Clear ownership
* Explicit interfaces
* Ubiquitous language consistency

---

# Bounded Context Overview

| Context             | Classification | Primary Responsibility                       |
| ------------------- | -------------- | -------------------------------------------- |
| Identity            | Generic        | Authentication, Authorization, User Identity |
| Rider               | Core           | Rider lifecycle management                   |
| Driver              | Core           | Driver lifecycle and onboarding              |
| Ride                | Core           | Ride lifecycle orchestration                 |
| Pricing             | Core           | Fare calculation and pricing policies        |
| Subscription        | Core           | Driver subscription lifecycle                |
| Payments            | Supporting     | Financial transactions                       |
| Notifications       | Supporting     | Customer communications                      |
| Support             | Supporting     | Customer issue resolution                    |
| Analytics           | Supporting     | Reporting, KPIs, operational insights        |
| Platform Operations | Generic        | Configuration, Feature Flags, Monitoring     |

---

# Context Details

## 1. Identity Context

### Purpose

Provide secure authentication and authorization across the platform.

### Responsibilities

* User registration
* Login
* MFA
* JWT issuance
* Role management
* Device trust
* Session management

### Owns

* User
* Role
* Permission
* Session

### Publishes Events

* UserRegistered
* UserAuthenticated
* UserLocked
* PasswordChanged

### Consumes Events

* DriverApproved
* RiderRegistered

---

## 2. Rider Context

### Purpose

Manage the complete lifecycle of riders.

### Responsibilities

* Profile management
* Preferences
* Saved locations
* Ride history
* Ratings

### Owns

* Rider
* RiderProfile
* SavedLocation

### Publishes

* RiderCreated
* RiderUpdated

---

## 3. Driver Context

### Purpose

Manage drivers from onboarding through active operations.

### Responsibilities

* Registration
* KYC
* Vehicle verification
* Driver status
* Earnings summary
* Performance metrics

### Owns

* Driver
* Vehicle
* DriverDocument
* DriverStatus

### Publishes

* DriverRegistered
* DriverVerified
* DriverActivated
* DriverSuspended

---

## 4. Ride Context

### Purpose

Coordinate the complete ride lifecycle.

### Responsibilities

* Ride requests
* Driver assignment
* Ride state transitions
* GPS tracking
* Ride completion
* Cancellation

### Aggregate Root

Ride

### Owns

* Ride
* Route
* RideTimeline

### Publishes

* RideRequested
* RideMatched
* RideAccepted
* RideStarted
* RideCompleted
* RideCancelled

This context represents the heart of RideFlow.

---

## 5. Pricing Context

### Purpose

Calculate fares using configurable business policies.

### Responsibilities

* Base fare
* Distance pricing
* Time pricing
* Surge pricing
* Coupons
* Promotions

### Owns

* FarePolicy
* Promotion
* Coupon

### Publishes

* FareCalculated

---

## 6. Subscription Context

### Purpose

Manage recurring subscriptions for drivers.

### Responsibilities

* Plan management
* Billing cycles
* Renewals
* Expiration
* Grace periods

### Owns

* Subscription
* Plan
* Invoice

### Publishes

* SubscriptionActivated
* SubscriptionExpired
* SubscriptionRenewed

---

## 7. Payments Context

### Purpose

Handle all financial transactions.

### Responsibilities

* Payment authorization
* Payment capture
* Refunds
* Settlement
* Wallet management

### Owns

* Payment
* Transaction
* Refund

### Publishes

* PaymentAuthorized
* PaymentCaptured
* RefundIssued

---

## 8. Notification Context

### Purpose

Deliver communications to users.

### Responsibilities

* Push notifications
* SMS
* Email
* In-app notifications

### Owns

* Notification
* Template
* DeliveryStatus

### Publishes

* NotificationDelivered
* NotificationFailed

---

## 9. Support Context

### Purpose

Resolve customer issues.

### Responsibilities

* Ticket lifecycle
* Customer communication
* Refund requests
* Escalations

### Owns

* SupportTicket
* Resolution
* Escalation

---

## 10. Analytics Context

### Purpose

Provide operational and business intelligence.

### Responsibilities

* KPIs
* Dashboards
* Reporting
* Fraud insights
* Business intelligence

### Owns

* Metrics
* Dashboard
* Report

---

## 11. Platform Operations Context

### Purpose

Support operational excellence.

### Responsibilities

* Feature flags
* Configuration
* Audit logs
* Health checks
* Platform settings

### Owns

* FeatureFlag
* Configuration
* AuditEntry

---

# Communication Between Contexts

Contexts communicate using published business events.

Examples:

RideRequested
→ Pricing calculates fare.

RideAccepted
→ Notifications inform rider.

RideCompleted
→ Payments initiate settlement.

PaymentCaptured
→ Analytics updates KPIs.

SubscriptionExpired
→ Driver context disables ride acceptance.

No bounded context accesses another context's internal data directly.

Communication occurs only through well-defined APIs or business events.

---

# Context Ownership

| Context             | Owning Team                 |
| ------------------- | --------------------------- |
| Identity            | Platform Team               |
| Rider               | Rider Experience Team       |
| Driver              | Driver Platform Team        |
| Ride                | Mobility Team               |
| Pricing             | Pricing Team                |
| Subscription        | Commerce Team               |
| Payments            | Finance Platform Team       |
| Notifications       | Communication Platform Team |
| Support             | Customer Success Team       |
| Analytics           | Data Platform Team          |
| Platform Operations | Platform Engineering Team   |

---

# Evolution Strategy

## Startup Phase

Implementation:

Modular Monolith

Each bounded context becomes an independent module within the same codebase.

---

## Growth Phase

Contexts remain modular but communicate increasingly through domain events.

---

## Scale Phase

Only contexts requiring independent scalability, deployment cadence, or organizational autonomy are extracted into microservices.

This decision is driven by business and operational needs—not by architectural fashion.

---

# Anti-Patterns

Avoid:

* Shared databases across contexts
* Direct table access
* Circular dependencies
* Shared domain models
* Chatty synchronous communication
* Premature microservice decomposition

---

# Architect's Lens

Questions

1. Which contexts are Core Domains?

2. Which contexts change together?

3. Which contexts require independent scalability?

4. Which contexts are likely to become the first microservices?

5. Which contexts should always remain platform services?

---

# Key Architectural Decisions

1. Business boundaries define bounded contexts.
2. Modules precede microservices.
3. Events are preferred over direct dependencies.
4. Each context owns its data.
5. Team ownership aligns with context ownership.
6. Evolution is incremental and driven by measurable business needs.

---

# Traceability

Business Capability
→ Bounded Context
→ Aggregate
→ Domain Events
→ APIs
→ Modules
→ Services (future)

This traceability ensures every technical boundary originates from a business boundary.

---

# Conclusion

The bounded contexts defined in this document establish the architectural structure of RideFlow.

They provide the foundation for:

* Context Maps
* Event Storming
* Modular Monolith design
* Future microservices
* Team organization
* API boundaries
* Data ownership
* Architecture Decision Records

These boundaries should evolve only when business requirements justify change, preserving architectural stability while allowing the platform to grow.
