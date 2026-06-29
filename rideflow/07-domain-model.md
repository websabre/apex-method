---
id: RF-DDD-002
title: RideFlow Domain Model
version: 1.0
status: Approved
owner: Solution Architecture Team
reviewers:
  - Domain Experts
  - Enterprise Architecture
last_updated: 2026-06-29
---

# RideFlow Domain Model

> "The domain model represents the business, not the database."

---

# Purpose

The RideFlow Domain Model identifies the core business concepts that define the RideFlow platform.

This model is independent of technology, programming language, databases, and infrastructure.

Its purpose is to establish a shared understanding of the business and provide the foundation for architecture, APIs, bounded contexts, and implementation.

---

# Domain Classification

Following Domain-Driven Design (DDD), RideFlow is divided into:

## Core Domains

These capabilities provide competitive advantage.

- Ride Management
- Driver Management
- Pricing
- Subscription

---

## Supporting Domains

Necessary for business operations.

- Payments
- Notifications
- Analytics
- Support

---

## Generic Domains

Cross-cutting concerns.

- Identity
- Audit
- Configuration
- Observability

---

# Aggregates

## Rider Aggregate

### Aggregate Root

Rider

### Entities

- Rider
- RiderProfile
- EmergencyContact

### Value Objects

- Address
- Rating
- PreferredPaymentMethod

---

## Driver Aggregate

### Aggregate Root

Driver

### Entities

- Driver
- Vehicle
- DriverDocument

### Value Objects

- License
- VehicleRegistration
- CurrentLocation

---

## Ride Aggregate

### Aggregate Root

Ride

### Entities

- Ride
- Route
- Fare
- RideTimeline

### Value Objects

- PickupLocation
- Destination
- FareBreakdown
- Distance
- ETA

---

## Payment Aggregate

### Aggregate Root

Payment

### Entities

- Transaction
- Refund

### Value Objects

- Money
- Currency
- PaymentReference

---

## Subscription Aggregate

### Aggregate Root

Subscription

### Entities

- Plan
- Renewal
- Invoice

### Value Objects

- BillingCycle
- SubscriptionStatus

---

# Domain Services

RideMatchingService

PricingService

DriverEligibilityService

SubscriptionService

FraudDetectionService

SettlementService

---

# Domain Events

RideRequested

DriverAssigned

RideAccepted

RideStarted

RideCompleted

RideCancelled

PaymentAuthorized

PaymentCaptured

PaymentRefunded

SubscriptionActivated

SubscriptionRenewed

DriverSuspended

---

# Policies

Surge Pricing Policy

Driver Assignment Policy

Cancellation Policy

Fraud Policy

Subscription Renewal Policy

---

# Invariants

Ride

- Must have one Rider.
- Must have one Driver after assignment.
- Cannot be completed twice.
- Cannot transition from Completed to In Progress.

Driver

- Must possess a valid subscription.
- Must have verified KYC.
- Must have an approved vehicle.

Payment

- Cannot be captured twice.
- Refund amount cannot exceed payment amount.

Subscription

- Only one active subscription per driver.
- Expired subscriptions prevent ride acceptance.

---

# Domain Relationships

Rider
    |
    | requests
    v
Ride
    |
    | assigned to
    v
Driver
    |
    | owns
    v
Vehicle

Ride
    |
    | generates
    v
Payment

Driver
    |
    | owns
    v
Subscription

---

# Future Evolution

The aggregates defined here will later become:

- Bounded Contexts
- APIs
- Event Streams
- Microservices (if justified)

The domain model remains stable even if implementation technologies evolve.