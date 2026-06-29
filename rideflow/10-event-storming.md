---

id: RF-DDD-005
title: RideFlow Event Storming
version: 1.0
status: Approved
owner: Solution Architecture Team
reviewers:

* Product Manager
* Domain Experts
* Enterprise Architecture
  last_updated: 2026-06-29

---

# RideFlow Event Storming

> *"Business events tell the story of the system. Architecture emerges from that story."*

---

# Purpose

This document captures the major business events, commands, actors, policies, aggregates, and integrations that define RideFlow.

Unlike sequence diagrams, Event Storming focuses on **business behavior** rather than technical implementation.

It provides a shared understanding of how the business operates and serves as the foundation for APIs, asynchronous messaging, CQRS, Sagas, and future microservice boundaries.

---

# Event Storming Method

RideFlow uses the following notation:

🟦 Actor

🟨 Command

🟧 Aggregate

🟪 Policy

🟩 Domain Event

🟥 External System

📘 Read Model

---

# Event Storming Principles

* Business events are immutable facts.
* Commands express intent.
* Aggregates enforce business rules.
* Policies react to events.
* External systems integrate through well-defined interfaces.
* Read models optimize queries without affecting transactional consistency.

---

# Business Process 1 – Rider Books a Ride

## Workflow

```text
🟦 Rider
      │
      ▼
🟨 Request Ride
      │
      ▼
🟧 Ride Aggregate
      │
      ▼
🟩 Ride Requested
      │
      ▼
🟪 Dispatch Policy
      │
      ▼
🟨 Find Eligible Drivers
      │
      ▼
🟧 Driver Aggregate
      │
      ▼
🟩 Driver Matched
      │
      ▼
🟨 Accept Ride
      │
      ▼
🟩 Ride Accepted
      │
      ▼
🟥 Notification Provider
      │
      ▼
🟩 Rider Notified
```

---

# Business Process 2 – Driver Onboarding

```text
Driver

↓

Register Driver

↓

Driver Aggregate

↓

Driver Registered

↓

Verify Documents

↓

KYC Provider

↓

Driver Verified

↓

Activate Subscription

↓

Subscription Aggregate

↓

Subscription Activated

↓

Driver Activated
```

---

# Business Process 3 – Ride Completion

```text
Ride Started

↓

Reach Destination

↓

Complete Ride

↓

Ride Completed

↓

Calculate Final Fare

↓

Capture Payment

↓

Payment Captured

↓

Update Earnings

↓

Send Receipt

↓

Update Analytics
```

---

# Commands

## Rider Commands

* Register Rider
* Login
* Request Ride
* Cancel Ride
* Rate Driver
* Add Payment Method

---

## Driver Commands

* Register Driver
* Upload Documents
* Accept Ride
* Reject Ride
* Start Ride
* Complete Ride

---

## Operations Commands

* Suspend Driver
* Approve Driver
* Issue Refund
* Resolve Ticket

---

# Domain Events

## Rider Events

* Rider Registered
* Rider Logged In
* Ride Requested
* Ride Cancelled

---

## Driver Events

* Driver Registered
* Driver Verified
* Driver Activated
* Driver Suspended

---

## Ride Events

* Driver Matched
* Ride Accepted
* Ride Started
* Ride Completed

---

## Payment Events

* Payment Authorized
* Payment Captured
* Refund Issued

---

## Subscription Events

* Subscription Activated
* Subscription Renewed
* Subscription Expired

---

# Aggregates

Ride

Driver

Rider

Payment

Subscription

Support Ticket

Each aggregate validates business rules before publishing domain events.

---

# Policies

## Dispatch Policy

When Ride Requested occurs:

* Find eligible drivers.
* Rank candidates.
* Notify the highest-ranked driver.

---

## Pricing Policy

When Ride Completed occurs:

* Calculate distance.
* Calculate duration.
* Apply surge pricing.
* Apply promotions.
* Produce final fare.

---

## Subscription Policy

When Subscription Expired occurs:

* Disable ride acceptance.
* Notify driver.
* Generate renewal reminder.

---

## Fraud Policy

When unusual ride behavior is detected:

* Flag transaction.
* Notify operations.
* Prevent settlement if necessary.

---

# Read Models

Ride History

Driver Dashboard

Fleet Dashboard

Operations Dashboard

Finance Dashboard

Customer Support Timeline

These models are optimized for querying and reporting and may be updated asynchronously.

---

# External Systems

Payment Gateway

Maps Provider

Push Notification Provider

SMS Gateway

Email Provider

ONDC (Future)

Government KYC Services

Cloud Monitoring Platform

---

# Business Rules

* A rider cannot have multiple active rides.
* A driver cannot accept a ride without an active subscription.
* Payments cannot be captured twice.
* A completed ride cannot be modified.
* Refunds require a completed payment.
* Driver KYC must be verified before activation.

---

# Hotspots

The following areas require careful architectural attention due to their complexity or business impact:

| Area               | Why It Matters                |
| ------------------ | ----------------------------- |
| Ride Matching      | Low latency, high scalability |
| Pricing Engine     | Business differentiation      |
| Payment Processing | Financial correctness         |
| Driver Onboarding  | Regulatory compliance         |
| Notifications      | High throughput               |
| Analytics          | Operational visibility        |

---

# Failure Scenarios

## Ride Matching Failure

Mitigation

* Retry strategy
* Driver fallback
* Timeout handling

---

## Payment Failure

Mitigation

* Idempotent payment processing
* Retry
* Compensation workflow

---

## Notification Failure

Mitigation

* Retry queue
* Dead Letter Queue
* Multiple notification channels

---

# Quality Attribute Mapping

| Event                  | Primary Quality Attribute |
| ---------------------- | ------------------------- |
| Ride Requested         | Performance               |
| Driver Matched         | Scalability               |
| Ride Accepted          | Availability              |
| Ride Completed         | Reliability               |
| Payment Captured       | Consistency               |
| Subscription Activated | Security                  |
| Notification Delivered | Reliability               |

---

# Event Ownership

| Event                  | Owning Context |
| ---------------------- | -------------- |
| Ride Requested         | Ride           |
| Driver Matched         | Ride           |
| Ride Accepted          | Ride           |
| Ride Started           | Ride           |
| Ride Completed         | Ride           |
| Payment Captured       | Payments       |
| Subscription Activated | Subscription   |
| Driver Verified        | Driver         |

The owning context is the authoritative publisher of each business event.

---

# Architect's Lens

Questions

1. Which events should be synchronous?

2. Which events should be asynchronous?

3. Which business events require exactly-once processing?

4. Which workflows are candidates for Saga orchestration?

5. Which events should be retained for auditing and analytics?

---

# Traceability

Business Goal
→ User Journey
→ Command
→ Aggregate
→ Domain Event
→ Policy
→ Read Model
→ Architecture
→ API
→ Deployment

Every event in RideFlow can be traced back to a business objective.

---

# Future Evolution

The Event Storming model will later generate:

* REST API Specifications
* AsyncAPI Specifications
* Domain Event Catalog
* Saga Definitions
* CQRS Commands
* Read Models
* Integration Events
* Sequence Diagrams
* Architecture Decision Records
* Distributed Tracing Strategy

---

# Conclusion

The RideFlow Event Storming model captures the dynamic behavior of the platform and establishes the foundation for event-driven architecture.

It provides a common language for business stakeholders, architects, developers, testers, and operations teams while preserving traceability from business intent to technical implementation.
