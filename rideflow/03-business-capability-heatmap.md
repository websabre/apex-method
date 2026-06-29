---
id: RF-BUS-002
title: RideFlow Business Capability Heat Map
version: 1.0
status: Draft
owner: Enterprise Architecture
last_updated: 2026-06-29
---

# RideFlow Business Capability Heat Map

## Purpose

A Business Capability Heat Map helps architects understand where the organization should invest its engineering effort.

Rather than treating all capabilities equally, we classify them according to their strategic importance.

This influences architecture, staffing, technology investment, and roadmap planning.

---

# Classification

Capabilities are grouped into three categories.

## Strategic

These capabilities differentiate RideFlow in the market.

Investment should prioritize innovation and continuous improvement.

## Core

These capabilities are essential to daily operations.

They require high reliability and operational excellence.

## Supporting

These capabilities are necessary but do not create competitive advantage.

Where appropriate, they should leverage managed services or third-party solutions.

---

# Capability Heat Map

| Business Capability | Category | Strategic Importance | Suggested Investment |
|---------------------|----------|----------------------|----------------------|
| Driver Management | Strategic | ★★★★★ | High |
| Ride Matching | Strategic | ★★★★★ | High |
| Pricing Engine | Strategic | ★★★★★ | High |
| Subscription Management | Strategic | ★★★★★ | High |
| Rider Management | Core | ★★★★☆ | Medium-High |
| Payments | Core | ★★★★☆ | High |
| Notifications | Core | ★★★☆☆ | Medium |
| Support | Core | ★★★☆☆ | Medium |
| Analytics | Core | ★★★★☆ | Medium-High |
| Identity & Security | Core | ★★★★★ | High |
| Fleet Management | Supporting (v1) | ★★☆☆☆ | Low |
| Marketing Campaigns | Supporting | ★★☆☆☆ | Low |
| CMS / Content | Supporting | ★☆☆☆☆ | Low |

---

# Architectural Implications

## Strategic Capabilities

Architectural focus:

- Domain-Driven Design
- Independent ownership
- High scalability
- Extensive testing
- Product innovation

Examples:

- Ride Matching
- Pricing
- Subscription

---

## Core Capabilities

Architectural focus:

- Reliability
- Security
- Operational excellence
- Observability
- Compliance

Examples:

- Payments
- Identity
- Analytics

---

## Supporting Capabilities

Architectural focus:

- Buy before Build
- SaaS integration
- Low maintenance
- Cost optimization

Examples:

- CMS
- Email Marketing
- Internal Wiki

---

# Technology Investment Strategy

Strategic capabilities justify custom engineering.

Core capabilities should balance custom development with proven platforms.

Supporting capabilities should prioritize simplicity and operational efficiency.

---

# Architect's Lens

Questions

1. Which capabilities create competitive advantage?

2. Which capabilities could be outsourced?

3. Which capabilities should never be outsourced?

4. If engineering resources are limited, where should investment begin?

---

# Future Evolution

As RideFlow grows, this heat map should be reviewed every six months.

Business priorities change.

Architecture should evolve with them.