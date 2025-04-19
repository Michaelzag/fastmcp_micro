---
order: 3
---

# Graph Ingestion Service (FastMCP)

_Proposed development order: 3_

# Overview

The Graph Ingestion Service is responsible for ingesting graph data from various sources and storing it in a graph database. This service will be built using FastMCP, a high-performance graph processing framework.

## Architecture

The Graph Ingestion Service will consist of the following components:

*   **Graph Ingestion API**: This API will be responsible for receiving graph data from various sources and sending it to the Graph Ingestion Service for processing.
*   **Graph Ingestion Service**: This service will be responsible for processing the graph data and storing it in a graph database.
*   **Graph Database**: This database will store the ingested graph data.

## Implementation

The Graph Ingestion Service will be implemented using FastMCP, a high-performance graph processing framework. The service will be designed to handle large volumes of graph data and will be optimized for performance.

## Technical Requirements

* Python 3.13+
* `neo4j`
* `fastmcp`, `pydantic`, `logfire`
* All dependencies managed by `uv` and listed in `pyproject.toml`

## Testing

The Graph Ingestion Service will be tested using a combination of unit tests and integration tests. The tests will be designed to ensure that the service is working correctly and that it can handle large volumes of graph data.

## Deployment

The Graph Ingestion Service will be deployed on a cloud-based infrastructure. The service will be designed to scale horizontally to handle large volumes of graph data.

### Step-by-Step Implementation Plan

#### 1. Project Setup
- Initialize the project using `uv init graph-ingestion-service` (Python 3.13+).
- Add dependencies: `uv add fastmcp pydantic neo4j logfire`.
- Use `main.py` as entry point, create `schemas.py` for Pydantic models, and `graph_client.py` for Neo4j logic.
- Store config and secrets (Neo4j credentials) in `.env` or environment variables.

#### 2. Schema and Contract Definition
- Define `GraphNode`, `GraphEdge`, and related schemas in `schemas.py` using Pydantic.
- Document expected input/output in the README and OpenAPI doc.

#### 3. Neo4j Integration
- Use official `neo4j` Python driver for DB operations.
- Implement connection pooling, retries, and error handling.
- Define Cypher queries for node/edge creation and queries.
- Handle schema migrations or upgrades if needed.

#### 4. MCP Interface Implementation
- Register `add_to_graph` and `query_graph` as FastMCP tools.
- Register `get_graph_node` as a FastMCP resource.
- Expose all as REST endpoints via FastAPI.

#### 5. Configuration & Environment
- Use `dotenv` or FastMCP config loader to read `.env`.
- Document required environment variables: `NEO4J_URI`, `NEO4J_USER`, `NEO4J_PASSWORD`.
- Provide a sample `.env.example` file.

#### 6. Testing
- Unit tests for graph client logic (mock Neo4j).
- Integration tests for FastMCP endpoints.
- Contract tests for schema compliance.
- Edge case tests: duplicate nodes, missing relationships, DB errors.
- Use `pytest` and `pytest-cov` for coverage.

#### 7. Observability & Logging
- Log all ingestion/query attempts and errors with `logfire`.
- Add metrics for query/ingestion latency and error rate.
- Provide log configuration in `pyproject.toml` or `.env`.

#### 8. Deployment
- Containerize with a `Dockerfile`.
- Expose port (default 8000) for FastAPI server.
- Document healthcheck endpoint (e.g., `/healthz`).
- Provide example `docker-compose.yml` for local dev (with Neo4j).

#### 9. Security & Compliance
- Never log sensitive data or secrets.
- Validate all inputs with Pydantic.
- Store secrets in env, not in code or logs.
- Document data retention policy for graph data.

#### 10. Maintenance & Future Work
- Document schema migration process.
- Instructions for scaling Neo4j or switching to managed service.
- Plan for evolving graph schema.
- Add monitoring hooks for production deployment.

---

### Example `.env.example`
```
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=secret
LOG_LEVEL=INFO
```

---

### Example API Calls

#### Add to Graph
`POST /tools/add_to_graph`
```json
{
  "node": {"type": "Order", "id": "123", "properties": {"amount": 42.0}}
}
```

#### Query Graph
`POST /tools/query_graph`
```json
{
  "cypher": "MATCH (n:Order) RETURN n LIMIT 10"
}
```

#### Get Graph Node
`GET /resources/get_graph_node/{node_id}`

---

### Troubleshooting Checklist
- [ ] Are all required env vars set? (`NEO4J_URI`, `NEO4J_USER`, `NEO4J_PASSWORD`)
- [ ] Is Neo4j reachable and running?
- [ ] Are FastMCP endpoints up and documented in OpenAPI?
- [ ] Are logs and metrics being generated?
- [ ] Do all contract tests pass?
