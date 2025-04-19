---
order: 7
---

# Orchestrator Service (FastMCP)

_Proposed development order: 7_

Orchestrator Service is a core component of the FastMCP system. It is responsible for managing the lifecycle of the services and applications running on the platform.

The Orchestrator Service provides the following features:

* Service discovery and registration
* Service instance management (e.g., start, stop, restart)
* Service scaling (e.g., scale up, scale down)
* Service monitoring and logging
* Service dependency management

The Orchestrator Service is designed to be highly available and scalable. It uses a distributed architecture to ensure that the system remains operational even in the event of node failures.

The Orchestrator Service is built using a microservices architecture, with each component communicating with others using RESTful APIs. This allows for a high degree of flexibility and customization.

The Orchestrator Service is responsible for managing the following components:

* Service Registry: responsible for storing information about available services
* Service Instances: responsible for managing the lifecycle of service instances
* Service Monitor: responsible for monitoring the health and performance of services

## Technical Requirements

* Python 3.13+
* `fastmcp`, `pydantic`, `logfire`, `fastapi`
* All dependencies managed by `uv` and listed in `pyproject.toml`

The Orchestrator Service has the following dependencies:

* Service Registry
* Service Instances
* Service Monitor

The Orchestrator Service provides the following APIs:

* Service registration API
* Service instance management API
* Service scaling API
* Service monitoring API

The Orchestrator Service has the following configuration options:

* Service registry URL
* Service instance management URL
* Service scaling URL
* Service monitoring URL

The Orchestrator Service has the following logging options:

* Log level (e.g., debug, info, warn, error)
* Log output (e.g., console, file)

The Orchestrator Service has the following security options:

* Authentication (e.g., username/password, OAuth)
* Authorization (e.g., role-based access control)

The Orchestrator Service is designed to be highly customizable. It provides a number of extension points that allow developers to add custom functionality.

The Orchestrator Service is built using a number of open-source components. It is designed to be highly modular and flexible.

The Orchestrator Service is responsible for managing the lifecycle of services and applications running on the platform. It provides a number of features, including service discovery and registration, service instance management, service scaling, service monitoring and logging, and service dependency management.

The Orchestrator Service is designed to be highly available and scalable. It uses a distributed architecture to ensure that the system remains operational even in the event of node failures.

The Orchestrator Service is built using a microservices architecture, with each component communicating with others using RESTful APIs. This allows for a high degree of flexibility and customization.

The Orchestrator Service is responsible for managing the following components:

* Service Registry: responsible for storing information about available services
* Service Instances: responsible for managing the lifecycle of service instances
* Service Monitor: responsible for monitoring the health and performance of services

The Orchestrator Service has the following dependencies:

* Service Registry
* Service Instances
* Service Monitor

The Orchestrator Service provides the following APIs:

* Service registration API
* Service instance management API
* Service scaling API
* Service monitoring API

The Orchestrator Service has the following configuration options:

* Service registry URL
* Service instance management URL
* Service scaling URL
* Service monitoring URL

The Orchestrator Service has the following logging options:

* Log level (e.g., debug, info, warn, error)
* Log output (e.g., console, file)

The Orchestrator Service has the following security options:

* Authentication (e.g., username/password, OAuth)
* Authorization (e.g., role-based access control)

The Orchestrator Service is designed to be highly customizable. It provides a number of extension points that allow developers to add custom functionality.

The Orchestrator Service is built using a number of open-source components. It is designed to be highly modular and flexible.

### Step-by-Step Implementation Plan

#### 1. Project Setup
- Initialize the project using `uv init orchestrator-service` (Python 3.13+).
- Add dependencies: `uv add fastmcp pydantic fastapi logfire`.
- Use `main.py` as entry point, create `schemas.py` for workflow/task models, and `orchestrator.py` for orchestration logic.
- Store config and secrets in `.env` or environment variables.

#### 2. Schema and Contract Definition
- Define workflow/task schemas in `schemas.py` using Pydantic.
- Document expected input/output in the README and OpenAPI doc.

#### 3. Workflow Logic Implementation
- Implement orchestration logic to call downstream FastMCP services in order.
- Add error handling, retries, and status tracking for each workflow step.
- Support workflow cancellation and status queries.

#### 4. MCP Interface Implementation
- Register workflow execution/status tools/resources as FastMCP endpoints.
- Expose all as REST endpoints via FastAPI.

#### 5. Configuration & Environment
- Use `dotenv` or FastMCP config loader to read `.env`.
- Document required environment variables for downstream service URLs, etc.
- Provide a sample `.env.example` file.

#### 6. Testing
- Unit tests for workflow logic (mock downstream services).
- Integration tests for FastMCP endpoints.
- Contract tests for schema compliance.
- Edge case tests: downstream service failure, partial workflow, invalid input.
- Use `pytest` and `pytest-cov` for coverage.

#### 7. Observability & Logging
- Log all workflow executions, errors, and downstream calls with `logfire`.
- Add metrics for workflow latency, error rate, and service usage.
- Provide log configuration in `pyproject.toml` or `.env`.

#### 8. Deployment
- Containerize with a `Dockerfile`.
- Expose port (default 8000) for FastAPI server.
- Document healthcheck endpoint (e.g., `/healthz`).
- Provide example `docker-compose.yml` for local dev.

#### 9. Security & Compliance
- Never log sensitive data or secrets.
- Validate all inputs with Pydantic.
- Store secrets in env, not in code or logs.
- Document data retention policy for workflow data.

#### 10. Maintenance & Future Work
- Document how to add new workflow steps/services.
- Instructions for updating service URLs or credentials.
- Plan for supporting dynamic workflows.
- Add monitoring hooks for production deployment.

---

### Example `.env.example`
```
GMAIL_SERVICE_URL=http://localhost:8001
CLASSIFIER_SERVICE_URL=http://localhost:8002
GRAPH_SERVICE_URL=http://localhost:8003
LOG_LEVEL=INFO
```

---

### Example API Calls

#### Start Workflow
`POST /tools/start_workflow`
```json
{
  "email_id": "abc123"
}
```

#### Get Workflow Status
`GET /resources/get_workflow_status/{workflow_id}`

---

### Troubleshooting Checklist
- [ ] Are all required env vars set? (service URLs, etc.)
- [ ] Are all downstream services reachable?
- [ ] Are FastMCP endpoints up and documented in OpenAPI?
- [ ] Are logs and metrics being generated?
- [ ] Do all contract tests pass?
