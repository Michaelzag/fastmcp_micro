# High-Level Package Selection for Agentic Workflow

This document provides a high-level overview of the kinds of packages and technologies you might consider for each component of the modular agentic workflow described in `fastmcp_agentic_architecture.md`. The goal is to outline the main categories and criteria for package selection, not to pick specific libraries yet.

---

## 1. Gmail Retrieval Service
- **Needs:** Gmail API access, OAuth2 authentication, incremental sync, error handling.
- **Package Types:**
  - Gmail/Google API client
  - OAuth2 authentication helpers
  - Async HTTP client (optional)

## 2. Classification & Caching Service
- **Needs:** Email parsing, classification (rules, ML, LLM), local cache or lightweight database.
- **Package Types:**
  - Email parsing utilities
  - ML/LLM inference (local or cloud)
  - Caching/database (SQLite, Redis, etc.)
  - Data serialization (JSON, Pickle, etc.)

## 3. Email Monitoring Orchestrator
- **Needs:** Scheduling, workflow orchestration, logging, error recovery.
- **Package Types:**
  - Task/job scheduler
  - Logging utilities
  - Workflow/orchestration frameworks (optional)

## 4. Graph Database Ingestion Service
- **Needs:** Graph database client, schema management, efficient ingest, query interface.
- **Package Types:**
  - Graph DB client (Neo4j, ArangoDB, etc.)
  - Data modeling/ORM
  - Query builder

## 5. Task Queue & Notification Service
- **Needs:** Queueing, notification (email, push, etc.), rules engine, persistence.
- **Package Types:**
  - Task/queue system (Redis, RabbitMQ, etc.)
  - Notification libraries (email, SMS, push)
  - Rules engine
  - Persistence layer

## 6. LLM Agent Service
- **Needs:** LLM API client, prompt management, agentic workflow, streaming support.
- **Package Types:**
  - LLM API/SDK (OpenAI, Azure, etc.)
  - Prompt templating
  - Agentic orchestration (optional)
  - Streaming output support

## 7. FastMCP Core
- **Needs:** MCP server/client, mounting, proxying, transport management.
- **Package Types:**
  - FastMCP (core package)
  - Optional: CLI helpers, deployment tools

---

## General Considerations
- **Async vs Sync:** Prefer async-compatible libraries for IO-heavy components.
- **Observability:** Choose packages with good logging, tracing, and error reporting.
- **Security:** Prioritize packages with active maintenance and security best practices.
- **Extensibility:** Favor libraries with plugin or extension support for future growth.

---

# Implementation-Ready Package Selection (Pydantic Ecosystem, Python 3.13 Compatible)

The following table presents a narrowed-down, opinionated selection of actively maintained packages for each component, prioritizing the Pydantic ecosystem wherever possible. All choices are Python 3.13 compatible and production-ready as of April 2025.

| Component | Need | Selected Package(s) | PyPI/Docs | Notes |
|-----------|------|---------------------|-----------|-------|
| **Agent Framework & Orchestration** | Agentic workflows, LLM integration, tool calling, type-safe outputs | `pydantic-ai` | [pydantic-ai](https://github.com/pydantic/pydantic-ai)<br>[Docs](https://ai.pydantic.dev/) | Modern, type-safe, model-agnostic agent framework by Pydantic team |
|  | Observability, tracing | `pydantic-logfire` | [pydantic-logfire](https://pydantic.dev/logfire/) | Deep integration with `pydantic-ai` for monitoring |
| **API Server** | ASGI server for FastAPI/agent apps | `granian` | [granian](https://github.com/emmett-framework/granian) | High-performance, production-ready ASGI/WSGI/RSGI server; use for dev & prod |
| **Web/API Framework** | API endpoints, validation, dependency injection | `FastAPI` | [FastAPI](https://fastapi.tiangolo.com/) | Built on Pydantic, seamless integration |
| **Graph Database** | Graph storage, knowledge graph | `neo4j` | [neo4j-py](https://pypi.org/project/neo4j/) | Official Python driver, 3.13 compatible |
| **Caching/Queue** | Lightweight caching, queueing | `sqlite3` (stdlib)<br>`redis` | [sqlite3 docs](https://docs.python.org/3/library/sqlite3.html)<br>[redis-py](https://redis.io/docs/latest/develop/clients/redis-py/) | Use stdlib where possible |
| **Email/Gmail Service** | Gmail API, OAuth2 | `google-api-python-client`<br>`Authlib` | [google-api-python-client](https://github.com/googleapis/google-api-python-client)<br>[Authlib](https://pypi.org/project/Authlib/) | Official Google client, supports 3.13 |
| **Notification** | Unified notifications | `notifiers` | [notifiers](https://pypi.org/project/notifiers/) | Email, SMS, push, etc. |

---

## Implementation Steps

1. **Adopt the above packages as the default for all new microservices and workflows.**
2. **Pin versions** in requirements files for reproducibility.
3. **Use `granian` for local development and production server.**
4. **Leverage `pydantic-ai` for all agentic logic, tool integration, and LLM orchestration.**
5. **Integrate `pydantic-logfire` for observability and debugging.**
6. **Prefer stdlib packages where possible for stability and long-term support.**
7. **Regularly audit package maintenance and Python 3.13 compatibility.**

---

## Notes
- All selected packages are actively maintained and have recent releases supporting Python 3.13 as of April 2025.
- The Pydantic ecosystem (`pydantic-ai`, `pydantic-logfire`, `FastAPI`) is prioritized for type safety, developer experience, and future extensibility.
- Granian is recommended for both development and production as the ASGI server for FastAPI/agentic apps.
- Content enrichment service package selection is deferred for now; revisit when requirements are finalized.
- If a package becomes unmaintained or loses 3.13 support, review alternatives and update this document.
