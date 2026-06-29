---
id: RF-DDD-001
title: RideFlow Ubiquitous Language
version: 1.0
status: Draft
owner: Domain Architecture Team
reviewers:
  - Product Manager
  - Solution Architect
  - Domain Experts
last_updated: 2026-06-29
---

# RideFlow Ubiquitous Language

## Purpose

A ubiquitous language establishes a shared vocabulary between business stakeholders and engineering teams.

Every architectural discussion, domain model, API, event, and codebase should use these terms consistently.

The goal is to eliminate ambiguity and improve communication across the organization.

---

# Core Business Terms

## Rider

A customer who requests transportation services through the RideFlow platform.

---

## Driver

An independent service provider who accepts and completes ride requests.

---

## Fleet Owner

An organization or individual responsible for managing one or more drivers and vehicles.

---

## Vehicle

A registered transport asset (bike, auto, cab, etc.) approved for service.

---

## Ride Request

A rider's request for transportation between a pickup and destination.

---

## Ride

The complete lifecycle of transportation, beginning with driver acceptance and ending with successful completion or cancellation.

---

## Dispatch

The process of assigning the most suitable driver to a ride request.

---

## Subscription

A recurring payment that grants a driver access to the RideFlow platform.

---

## Fare

The amount charged for a completed ride.

---

## Wallet

A digital balance used for payments, refunds, promotions, or incentives.

---

## Incentive

A reward granted to drivers or riders based on predefined business rules.

---

## Promotion

A temporary offer intended to influence rider or driver behavior.

---

## Cancellation

Termination of a ride before completion by either the rider, driver, or the platform.

---

## Settlement

The financial reconciliation process between RideFlow and its partners.

---

## Service Area

A geographic region where RideFlow operates.

---

## Surge Pricing

A temporary fare adjustment based on demand, supply, or business policies.

---

# Business Events

Examples include:

- Ride Requested
- Driver Matched
- Driver Accepted
- Ride Started
- Ride Completed
- Ride Cancelled
- Payment Authorized
- Payment Settled
- Subscription Activated
- Driver Suspended

These events describe business behavior rather than technical implementation.

---

# Naming Principles

Every new term introduced into RideFlow should:

- Represent a real business concept.
- Be understandable by business and technical teams.
- Avoid technical jargon.
- Remain stable over time.
- Have a single agreed meaning.

---

# Architect's Lens

Questions

1. Which terms are most likely to be misunderstood?
2. Are there duplicate terms describing the same concept?
3. Does every event reflect a meaningful business occurrence?
4. Could a non-technical stakeholder understand this vocabulary?

---

# Decision

This ubiquitous language is the authoritative vocabulary for RideFlow.

All future documentation, APIs, domain models, ADRs, and source code should align with these definitions.