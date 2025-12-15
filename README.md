# Athomic 

[![Build Status](https://img.shields.io/github/actions/workflow/status/guandaline/athomic-docs/ci.yml?branch=main)](https://github.com/guandaline/athomic-docs/actions)
[![Coverage](https://img.shields.io/codecov/c/github/guandaline/athomic-docs.svg)](https://codecov.io/gh/guandaline/athomic-docs)
[![License](https://img.shields.io/github/license/guandaline/athomic-docs)](https://github.com/guandaline/athomic-docs/blob/main/LICENSE)
[![Python Version](https://img.shields.io/pypi/pyversions/athomic-docs.svg)](https://pypi.org/project/athomic-docs/)

**Athomic** is an opinionated, production-ready engine for building resilient, observable, and scalable microservices in Python. It provides a robust foundation—the **Athomic Layer**—that handles cross-cutting concerns, allowing developers to focus purely on business logic.

Our philosophy is grounded in battle-tested software engineering principles like **SOLID**, **Single Responsibility (SRP)**, and **Dependency Injection (DI)**, ensuring that applications built on this engine are maintainable, scalable, and easy to test.

---

## 🚀 Features & The "Athomic Layer"

Athomic provides a comprehensive set of modules known as the **Athomic Layer**, designed to handle the complexities of modern distributed systems.

### 🧠 AI & LLM Integration
* **Agents & RAG:** Built-in support for building AI agents and Retrieval-Augmented Generation systems.
* **LLM Abstraction:** Agnostic interfaces for various LLM providers.
* **Document Ingestion:** Pipelines for ingesting and processing documents for vector search.
* **Memory & Governance:** State management for AI conversations and governance policies.

### 💾 Data & Persistence
* **Connection Management:** Centralized lifecycle management for all data store connections.
* **Document Stores:** Robust abstraction for document databases (e.g., MongoDB) with **Beanie** ODM integration.
* **Key-Value Stores:** High-performance caching and storage (e.g., Redis) with flexible wrappers.
* **Graph Databases:** Asynchronous graph database support (e.g., Neo4j).
* **Vector Stores:** Integration with vector databases (e.g., Qdrant) for AI applications.
* **Transactional Outbox:** Guaranteed at-least-once event delivery using the Outbox Pattern.
* **Migrations:** Database-agnostic migration engine with distributed locking support.

### 🛡️ Security
* **Authentication & Authorization:** Policy-based security using JWT and API Keys.
* **Secrets Management:** Secure runtime secret resolution from providers like Vault.
* **Cryptography:** High-level abstractions for symmetric encryption.

### 👁️ Observability
* **Structured Logging:** JSON-based structured logging with automatic sensitive data masking.
* **Distributed Tracing:** End-to-end tracing powered by **OpenTelemetry**.
* **Metrics:** Out-of-the-box instrumentation with **Prometheus**.
* **Health Checks:** Extensible readiness and liveness probes for Kubernetes.
* **Data Lineage:** Tracking data flow and dependencies (OpenLineage support).

### ⚡ Performance & Resilience
* **Resilience Patterns:** Comprehensive implementation of patterns like **Retry**, **Circuit Breaker**, **Rate Limiter**, **Bulkhead**, **Timeout**, **Fallback**, and **Backpressure**.
* **Caching:** Decorator-based caching strategies.
* **High Performance:** Optimized bootstrap using `uvloop`.

### 🧩 Integration
* **Messaging:** Unified producer/consumer interfaces for message brokers (e.g., Kafka).
* **Service Discovery:** Integration with tools like Consul.
* **Feature Flags:** Dynamic feature toggling.

---

## 🏗️ Architecture

The project follows a clean, layered architecture to enforce separation of concerns:

* **Athomic Layer:** The infrastructure and utility layer provided by this framework.
* **Domain Layer:** Your business logic, completely decoupled from infrastructure details.
* **API Layer:** The entry point (FastAPI) that orchestrates the domain and athomic layers.

Dependencies are managed via a robust **Dependency Injection** system, making components loosely coupled and highly testable.

---

## 🛠️ Getting Started

Follow these steps to get a local development environment up and running.

### Prerequisites

* **Docker** & **Docker Compose**
* **Python 3.11+**
* **Poetry**

### 1. Clone the Repository

\```bash
git clone [https://github.com/guandaline/athomic-docs.git](https://github.com/guandaline/athomic-docs.git)
cd athomic-docs
\```

### 2. Start Infrastructure Services

This command will start all necessary backing services defined in `docker-compose.yml` (e.g., MongoDB, Redis, Vault, Neo4j, Qdrant).

\```bash
docker-compose up -d
\```

### 3. Install Dependencies

Install the project's Python dependencies using Poetry.

\```bash
poetry install
\```

### 4. Run the Application

Execute the application using `uvicorn`.

\```bash
poetry run uvicorn nala.api.main:app --reload
\```

The server will start on `http://127.0.0.1:8000`. You can access the interactive API documentation at:
* **Swagger UI:** `http://127.0.0.1:8000/docs`
* **ReDoc:** `http://127.0.0.1:8000/redoc`

---

## ⚙️ Configuration

Athomic uses a tiered configuration system supported by `Dynaconf` and `Pydantic`.

1.  **`settings.toml`**: Base configuration file.
2.  **Environment Variables**: Override any setting using the `NALA_` prefix (e.g., `NALA_DATABASE__DOCUMENTS__DEFAULT__URI`).
3.  **Secrets**: Sensitive values are resolved at runtime via the `SecretsManager`.

Example `settings.toml` snippet:

\```toml
[default.database.documents.default_mongo]
enabled = true
backend = "mongo"
  [default.database.documents.default_mongo.provider]
  database_name = "nala_db"
  # Secure password reference
  password = { path = "database/mongo", key = "password" }
\```

---

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md) to learn about our development process, how to propose bug fixes and improvements, and how to build and test your changes to Athomic.

## 📄 License

This project is licensed under the terms of the MIT license.