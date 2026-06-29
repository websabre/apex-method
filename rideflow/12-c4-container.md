---

id: RF-C4-002
title: RideFlow C4 Model - Level 2 - Container Diagram
version: 1.0
status: Approved
owner: Solution Architecture Team
last_updated: 2026-06-29
------------------------

# RideFlow C4 Model — Level 2 (Container Diagram)

> *"Containers describe deployable applications and data stores—not infrastructure."*

---

# Purpose

The Container Diagram decomposes the RideFlow Platform into its major deployable applications and persistent stores while preserving the logical boundaries established by the bounded contexts.

For Version 1.0, RideFlow is implemented as a **Modular Monolith**. Although many business modules exist internally, they are deployed as a single application.

---

# Containers

| Container              | Technology                           | Responsibility                      |
| ---------------------- | ------------------------------------ | ----------------------------------- |
| Rider Mobile App       | Flutter / React Native               | Rider experience                    |
| Driver Mobile App      | Flutter / React Native               | Driver experience                   |
| Admin Portal           | React                                | Internal administration             |
| RideFlow API           | ASP.NET Core (.NET LTS)              | Business APIs and application logic |
| PostgreSQL             | PostgreSQL                           | Transactional data                  |
| Redis                  | Redis                                | Cache and transient state           |
| Object Storage         | Azure Blob Storage / S3              | Documents and media                 |
| Observability Platform | OpenTelemetry + Prometheus + Grafana | Logs, metrics, traces               |

---

# Container Diagram

```mermaid
graph TB

    Rider[Rider Mobile App]
    Driver[Driver Mobile App]
    Admin[Admin Portal]

    API[RideFlow API\n(Modular Monolith)]

    DB[(PostgreSQL)]
    Redis[(Redis)]
    Blob[(Object Storage)]

    Obs[Observability Platform]

    Rider --> API
    Driver --> API
    Admin --> API

    API --> DB
    API --> Redis
    API --> Blob
    API --> Obs
```

---

# Container Responsibilities

## Mobile Applications

Provide user interfaces and interact with RideFlow through secure REST APIs.

## RideFlow API

Hosts all business modules:

* Identity
* Rider
* Driver
* Ride
* Pricing
* Payments
* Subscription
* Notifications
* Support
* Analytics

These remain logically separated but are deployed together.

## PostgreSQL

Stores transactional business data.

## Redis

Supports caching, session optimization, rate limiting, and transient application state.

## Object Storage

Stores KYC documents, receipts, and uploaded media.

## Observability Platform

Collects logs, metrics, traces, and health information.

---

# Key Decisions

* Single deployable application.
* Logical module isolation.
* Shared process, separate domain boundaries.
* Independent databases are not introduced until business evolution justifies them.

---

# Evolution

As RideFlow grows, selected modules may evolve into independently deployable services without changing the business model or bounded contexts.
