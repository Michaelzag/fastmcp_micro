---
order: 5
---

# LLM Agent Service (FastMCP)

_Proposed development order: 5_

# Overview

The LLM Agent Service is a key component of the FastMCP architecture, responsible for providing a unified interface to various large language models (LLMs). This service enables the integration of multiple LLMs, allowing the system to leverage their strengths and provide more accurate and informative responses.

## Architecture

The LLM Agent Service is designed as a microservice, communicating with other components of the FastMCP system through RESTful APIs. The service is responsible for:

*   Managing the lifecycle of LLM instances, including initialization, configuration, and termination.
*   Providing a unified API for querying LLMs, abstracting away the underlying model-specific details.
*   Handling requests from other components, such as the Dialogue Manager and the Knowledge Graph Service.
*   Caching and optimizing responses to improve performance and reduce latency.

## Implementation

The LLM Agent Service is implemented using a combination of Python and containerization technologies (e.g., Docker). The service utilizes a modular architecture, allowing for easy integration of new LLMs and customization of existing ones.

### Technical Requirements

* Python 3.13+
* `openai` or `pydantic-ai`
* `fastmcp`, `pydantic`, `logfire`
* All dependencies managed by `uv` and listed in `pyproject.toml`

### LLM Integration

To integrate a new LLM, developers can create a custom module that implements the required interfaces and APIs. This module can then be registered with the LLM Agent Service, making the new LLM available to the rest of the system.

### API Documentation

The LLM Agent Service provides a comprehensive API documentation, detailing the available endpoints, request and response formats, and error handling mechanisms.

### Example Usage

To demonstrate the usage of the LLM Agent Service, consider the following example:

*   A user submits a query to the Dialogue Manager, which in turn sends a request to the LLM Agent Service to retrieve a response from a specific LLM.
*   The LLM Agent Service receives the request, identifies the target LLM, and forwards the query to the corresponding LLM instance.
*   The LLM instance processes the query and returns a response to the LLM Agent Service.
*   The LLM Agent Service caches the response and returns it to the Dialogue Manager, which then presents the response to the user.

### Step-by-Step Implementation Plan

#### 1. Project Setup
- Initialize the project using `uv init llm-agent-service` (Python 3.13+).
- Add dependencies: `uv add fastmcp pydantic openai logfire`.
- Use `main.py` as entry point, create `schemas.py` for Pydantic models, and `llm_client.py` for LLM logic.
- Store config and secrets (OpenAI API key) in `.env` or environment variables.

#### 2. Schema and Contract Definition
- Define `AgentTask`, `AgentResult`, and related schemas in `schemas.py` using Pydantic.
- Document expected input/output in the README and OpenAPI doc.

#### 3. LLM Integration
- Integrate with OpenAI or pydantic-ai for LLM tasks.
- Implement prompt templates and task routing.
- Add support for multiple LLM providers if needed.
- Handle API errors, retries, and quota limits.

#### 4. MCP Interface Implementation
- Register `run_agent_task` as a FastMCP tool (input: `AgentTask`, output: `AgentResult`).
- Register `get_agent_result` as a FastMCP resource.
- Expose both as REST endpoints via FastAPI.

#### 5. Configuration & Environment
- Use `dotenv` or FastMCP config loader to read `.env`.
- Document required environment variables: `OPENAI_API_KEY`, etc.
- Provide a sample `.env.example` file.

#### 6. Testing
- Unit tests for LLM client logic (mock OpenAI API).
- Integration tests for FastMCP endpoints (`run_agent_task`, `get_agent_result`).
- Contract tests for schema compliance.
- Edge case tests: empty prompt, invalid input, API errors.
- Use `pytest` and `pytest-cov` for coverage.

#### 7. Observability & Logging
- Log all agentic tasks, errors, and API usage with `logfire`.
- Add metrics for task latency, error rate, and provider usage.
- Provide log configuration in `pyproject.toml` or `.env`.

#### 8. Deployment
- Containerize with a `Dockerfile`.
- Expose port (default 8000) for FastAPI server.
- Document healthcheck endpoint (e.g., `/healthz`).
- Provide example `docker-compose.yml` for local dev.

#### 9. Security & Compliance
- Never log prompt content or secrets.
- Validate all inputs with Pydantic.
- Store secrets in env, not in code or logs.
- Document data retention policy for agent results.

#### 10. Maintenance & Future Work
- Document how to add new LLM providers or prompt templates.
- Instructions for prompt versioning and migration.
- Plan for supporting more advanced LLM features.
- Add monitoring hooks for production deployment.

### Example `.env.example`
```
OPENAI_API_KEY=sk-...
LOG_LEVEL=INFO
```

### Example API Calls

#### Run Agent Task
`POST /tools/run_agent_task`
```json
{
  "prompt": "Summarize this email...",
  "context": {"email_id": "123"}
}
```

#### Get Agent Result
`GET /resources/get_agent_result/{task_id}`

### Troubleshooting Checklist
- [ ] Are all required env vars set? (`OPENAI_API_KEY`)
- [ ] Is the LLM API reachable and responding?
- [ ] Are FastMCP endpoints up and documented in OpenAPI?
- [ ] Are logs and metrics being generated?
- [ ] Do all contract tests pass?
