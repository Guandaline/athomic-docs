# Nala Core API (Fortify Framework)

[![Build Status](https://img.shields.io/github/actions/workflow/status/guandaline/nala-core-api/ci.yml?branch=main)](https://github.com/guandaline/nala-core-api/actions)
[![Coverage](https://img.shields.io/codecov/c/github/guandaline/nala-core-api.svg)](https://codecov.io/gh/guandaline/nala-core-api)
[![License](https://img.shields.io/github/license/guandaline/nala-core-api)](./LICENSE)
[![Commit Style](https://img.shields.io/badge/commit%20style-conventional-blue.svg)](./COMMIT_GUIDE.md)

**Nala Core API** is an opinionated, production-ready application chassis designed in Python to build resilient, observable, and secure microservices. It provides a robust foundation—the **Fortify Layer**—that handles cross-cutting infrastructure concerns, allowing engineering teams to focus exclusively on delivering business value.

## 🎯 Core Philosophy

The framework's architecture is built on the principle of **inverting control from the infrastructure to the business logic**. By providing a rich set of production-grade components as a service, Fortify accelerates development, enforces best practices, and drastically reduces the cognitive load on developers. Our design is guided by:

-   **Layered & Domain-Driven Design**: A strict separation between Domain, Application, and Infrastructure layers ensures your business logic remains pure and technology-agnostic.
-   **Protocol-Oriented Contracts**: Components are decoupled through abstract protocols, making the framework extensible and easy to test.
-   **Resilience & Security by Default**: We believe that a reliable and secure application is not an afterthought. These qualities are baked into the core of the framework.

---

## ✨ Feature Spotlight: Production-Ready Capabilities

While the framework is comprehensive, three core pillars exemplify the value Fortify delivers out-of-the-box.

### 1. Unshakeable Resilience & Fault Tolerance

Fortify provides a complete, cohesive suite of over a dozen resilience patterns that work together to create self-healing systems. It's not just a collection of tools; it's an integrated ecosystem.

-   **Proactive Failure Prevention**: The **Circuit Breaker**, **Bulkhead**, and **Rate Limiter** patterns protect your services from being overwhelmed by failing or slow dependencies. The state for these components can be distributed (via Redis), making them effective across a whole cluster of service instances.
-   **Automated Recovery**: The **Retry** pattern, with exponential backoff and jitter, handles transient failures automatically. For more complex scenarios, the **Fallback** mechanism allows for graceful degradation, serving stale cache data or default values instead of failing completely.
-   **Data Consistency in a Distributed World**: We provide advanced patterns like **Distributed Locking**, **Idempotency** for safe API retries, and a full-fledged **Saga** implementation (with both Orchestration and Choreography) to manage complex, multi-service transactions without distributed ACID.

### 2. Deep, Correlated Observability

Understanding a distributed system is impossible without first-class observability. Fortify provides a "three pillars" solution where logs, metrics, and traces are automatically correlated, giving you a unified view of every request.

-   **Structured Logging with Zero PII Leakage**: Our `Loguru`-based logging is not only structured (JSON) but also features a powerful, extensible engine that **automatically masks sensitive data and PII** before a log is ever written.
-   **End-to-End Distributed Tracing**: Built on the **OpenTelemetry** standard, tracing is automatic for all database calls, HTTP requests, and cache operations. Our most powerful feature is **seamless context propagation** across message brokers, meaning a single trace can span from an initial API request to the background worker that processes its resulting event.
-   **Rich Prometheus Metrics**: Nearly every component in Fortify is instrumented with detailed Prometheus metrics: API latency, database query times, Kafka consumer lag, circuit breaker state changes, and more. This provides immediate, deep insight into your application's health and performance.

### 3. Guaranteed Messaging with the Transactional Outbox

Fortify solves the critical "dual-write" problem in microservices with a powerful, horizontally scalable implementation of the **Transactional Outbox** pattern.

-   **Atomic & Reliable**: Events are saved to your primary database within the *same transaction* as your business data. This guarantees that a message is queued for delivery **if and only if** your business transaction succeeds.
-   **Distributed & Scalable Publisher**: A dedicated, multi-instance background service (the `OutboxPublisher`) reliably polls for new events and publishes them to the message broker.
-   **Intelligent Coordination**: The publisher uses other Fortify patterns like **Sharding** (to distribute the workload), **Leasing** (to guarantee event ordering per entity), and **Backpressure** (to avoid hammering a failing broker) to operate safely and efficiently at scale.

---

## 🗺️ Framework Modules at a Glance

This is a map of the capabilities provided by the Fortify Layer.

#### Core & Lifecycle
-   **Lifecycle Management**: Orchestrates the startup and shutdown sequence of all services.
-   **Plugin System**: Extends the application with new features (like API routes) in a decoupled way.
-   **Services**: A base protocol for all stateful, long-running components.

#### Configuration & Context
-   **Configuration**: A type-safe system combining `Dynaconf` and `Pydantic` for validated, environment-aware settings.
-   **Live Configuration**: A hot-reloading system to change application settings at runtime without restarts.
-   **Context Management**: Manages request-scoped context (`trace_id`, `tenant_id`) using `contextvars` for seamless propagation.

#### Data & Persistence
-   **Connection Management**: A central, lifecycle-managed registry for all database and cache connections.
-   **Document & Key-Value Stores**: Abstractions for MongoDB and Redis with a powerful wrapper system.
-   **Transactional Outbox**: Guarantees at-least-once event delivery.
-   **Database Migrations**: A programmatic, version-controlled schema migration engine.
-   **File Storage**: A clean abstraction for object storage (GCS, local files).

#### Security
-   **Secrets Management**: Just-in-time secret resolution from backends like HashiCorp Vault.
-   **Authentication & Authorization**: A flexible, policy-based system for securing endpoints (JWT, API Keys).
-   **Cryptography**: High-level abstraction for symmetric payload encryption (Fernet).

#### Cross-Cutting Concerns & Control
-   **Payload Pipeline**: A "Pipes and Filters" system for composing transformations like encryption and compression.
-   **Notifications**: A resilient system for sending emails (SMTP) and other notifications.
-   **Internal Event Bus**: An in-process Pub/Sub for intra-service communication.
-   **HTTP Client**: A factory for creating pre-configured, resilient, and observable HTTP clients.
-   **Feature Flags**: A system for dynamically toggling features at runtime.
-   **Task Scheduler**: A persistent, distributed scheduler for future or delayed task execution.

#### Integration Layer
-   **Messaging (Producers & Consumers)**: A high-level abstraction for Kafka with decorator-based consumers.
-   **Background Tasks**: An abstraction for enqueuing jobs to distributed task brokers like Taskiq or RQ.
-   **Service Discovery**: An integration with Consul for service registration and discovery.
-   **Data Lineage**: Generates and stores lineage events compliant with the **OpenLineage** standard.

---

## 📚 Documentation

For a deep dive into each module, architectural decision records (ADRs), and usage guides, please visit our **[full documentation site](https://guandaline.github.io/page-docs/)**.

## 🤝 Contributing

We welcome contributions! Please read our [**Contributing Guide**](./CONTRIBUTING.md) to learn about our development process, coding standards, and how to submit pull requests.

## 📜 License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
