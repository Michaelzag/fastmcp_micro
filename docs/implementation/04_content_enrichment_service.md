---
order: 4
---

# Content Enrichment Service (FastMCP)

_Proposed development order: 4_

# Overview

The Content Enrichment Service is a key component of the FastMCP platform, responsible for enhancing and augmenting the content provided by the Content Ingestion Service. This service will leverage various enrichment techniques, such as entity recognition, sentiment analysis, and topic modeling, to provide a more comprehensive understanding of the content.

## Architecture

The Content Enrichment Service will be built using a microservices architecture, with each enrichment technique implemented as a separate service. This will allow for greater flexibility and scalability, as new enrichment techniques can be added or removed as needed.

## Enrichment Techniques

The following enrichment techniques will be implemented as part of the Content Enrichment Service:

* Entity recognition: This technique will be used to identify and extract specific entities from the content, such as names, locations, and organizations.
* Sentiment analysis: This technique will be used to analyze the sentiment of the content, determining whether it is positive, negative, or neutral.
* Topic modeling: This technique will be used to identify the underlying topics or themes present in the content.

## Implementation

The Content Enrichment Service will be implemented using a combination of open-source and proprietary technologies. The specific technologies used will be determined based on their ability to meet the requirements of the service.

## Technical Requirements

* Python 3.13+
* `httpx`
* `fastmcp`, `pydantic`, `logfire`
* All dependencies managed by `uv` and listed in `pyproject.toml`

## Testing

The Content Enrichment Service will be thoroughly tested to ensure that it is functioning correctly and providing accurate results. This will include unit testing, integration testing, and performance testing.

## Deployment

The Content Enrichment Service will be deployed as part of the larger FastMCP platform. It will be designed to scale horizontally, allowing it to handle large volumes of content and traffic.

## Security

The Content Enrichment Service will be designed with security in mind, incorporating features such as authentication, authorization, and encryption to protect sensitive data.

## Monitoring and Maintenance

The Content Enrichment Service will be continuously monitored to ensure that it is functioning correctly and providing accurate results. Regular maintenance will be performed to update the service and ensure that it remains secure and scalable.

### Step-by-Step Implementation Plan

#### 1. Project Setup
- Initialize the project using `uv init content-enrichment-service` (Python 3.13+).
- Add dependencies: `uv add fastmcp pydantic httpx logfire`.
- Use `main.py` as entry point, create `schemas.py` for Pydantic models, and `plugins/` directory for enrichment plugins.
- Store config and secrets in `.env` or environment variables.

#### 2. Schema and Contract Definition
- Define `GraphNode`, `EnrichedNode`, and related schemas in `schemas.py` using Pydantic.
- Document expected input/output in the README and OpenAPI doc.

#### 3. Plugin System Implementation
- Design plugin interface (base class or protocol).
- Implement dynamic plugin loading (e.g., with `importlib`).
- Provide at least one sample enrichment plugin (e.g., entity recognition).
- Document how to add new plugins.

#### 4. MCP Interface Implementation
- Register `enrich_content` as a FastMCP tool.
- Register `get_enriched_node` as a FastMCP resource.
- Expose both as REST endpoints via FastAPI.

#### 5. Configuration & Environment
- Use `dotenv` or FastMCP config loader to read `.env`.
- Document required environment variables: plugin config, API keys, etc.
- Provide a sample `.env.example` file.

#### 6. Testing
- Unit tests for plugin logic and base interface.
- Integration tests for enrichment pipeline.
- Contract tests for schema compliance.
- Edge case tests: missing plugin, plugin errors, invalid data.
- Use `pytest` and `pytest-cov` for coverage.

#### 7. Observability & Logging
- Log all enrichment attempts, plugin loads/errors with `logfire`.
- Add metrics for enrichment latency, error rate, and plugin usage.
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
- Document data retention policy for enrichment data.

#### 10. Maintenance & Future Work
- Document how to add or update plugins.
- Instructions for plugin versioning and migration.
- Plan for plugin registry or discovery.
- Add monitoring hooks for production deployment.

---

### Example `.env.example`
```
PLUGIN_PATH=plugins/
ENTITY_API_KEY=...
LOG_LEVEL=INFO
```

---

### Example API Calls

#### Enrich Content
`POST /tools/enrich_content`
```json
{
  "node_id": "123",
  "plugin": "entity_recognition"
}
```

#### Get Enriched Node
`GET /resources/get_enriched_node/{node_id}`

---

### Troubleshooting Checklist
- [ ] Are all required env vars set? (`PLUGIN_PATH`, `ENTITY_API_KEY`)
- [ ] Are plugins present and loadable?
- [ ] Are FastMCP endpoints up and documented in OpenAPI?
- [ ] Are logs and metrics being generated?
- [ ] Do all contract tests pass?
