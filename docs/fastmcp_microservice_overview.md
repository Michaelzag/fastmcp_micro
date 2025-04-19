# FastMCP Microservice Overview & Implementation Guidelines

## Purpose
This document provides a high-level overview and the core rules for creating FastMCP-based microservices. It ensures all services in the FastMCP ecosystem are consistent, maintainable, and interoperable.

---

## FastMCP Microservice Principles
- **Isolation:** Each service is a standalone FastMCP server, running in its own process/container. No direct DB/filesystem access except for config/secrets.
- **Explicit Interface:** All functionality is exposed via the documented MCP interface (tools, resources, prompts).
- **REST API:** Every MCP tool/resource is also exposed as a REST endpoint via FastAPI, following OpenAPI conventions.
- **Statelessness:** Services should be stateless except for managed state (e.g., cache, queue).
- **Observability:** All services must log operations and errors, and expose basic metrics (e.g., latency, error rate).
- **Security:** No sensitive data in logs. Secrets/configs are stored securely (env or secret manager).
- **Testing:** Each service must include unit, integration, and contract tests.
- **Versioning:** Use semantic versioning (semver). Contract changes require a major version bump and contract test update.

---

## Microservice Implementation Steps
1. **Define the Service Goal:**
   - What does this service do? What MCP tools/resources/prompts does it expose?
2. **Specify the MCP Interface:**
   - List all tools, resources, and prompts, with signatures, input/output schemas, and examples.
3. **Dependencies:**
   - List required Python packages and external services.
4. **Security & Compliance:**
   - Specify how secrets are managed and what data is logged.
5. **Testing Plan:**
   - Unit: logic in isolation. Integration: FastMCP server with real/mocked clients. Contract: schema compliance.
6. **Observability:**
   - What logs and metrics are produced?
7. **REST API Exposure:**
   - Ensure all MCP endpoints are exposed as REST endpoints.
8. **Deployment:**
   - Each service should be containerized (Docker recommended).

---

## Example Service Doc Template
```
# [Service Name] (FastMCP)

## Service Name
[Service Name]

## Goal
[What does the service do?]

## MCP Interface
- **Tools:**
  - `tool_name()`
    - Signature: `def tool_name(self, arg: type) -> ReturnType`
    - Description: ...
    - Input schema: ...
    - Output schema: ...
    - Example call: ...
- **Resources:**
  - `resource://{id}`
    - Signature: ...
    - Description: ...
    - Output schema: ...
    - Example URI: ...
- **Prompts:** (if any)
  - ...

## REST API Exposure
- All MCP tools/resources/prompts are exposed as REST endpoints via FastAPI.
- Documented via OpenAPI.

## Inputs
[List all inputs: tool calls, resource fetches, environment, etc.]

## Outputs
[List all outputs: tool returns, resources, logs, etc.]

## Testing
- Unit: ...
- Integration: ...
- Contract: ...
- Example: ...

## Isolation
- Runs as standalone FastMCP server
- No direct DB/filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface

## Dependencies
- Python packages: ...
- MCP runtime

## Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

## Security & Compliance
- ...

## Observability
- ...

## References
- ...
```

---

## Rules for Contributors
- Follow the above template for every new FastMCP microservice.
- Use the narrowed-down, approved package list for dependencies.
- All code must be type-annotated and use Pydantic for schemas.
- All endpoints must be covered by tests.
- Document any deviations from these rules in the service's README.

---

## References
- [FastMCP Agentic Architecture](./fastmcp_agentic_architecture.md)
- [FastMCP Spec](./fastmcp_spec.md)
- [FastMCP Agentic Packages](./fastmcp_agentic_packages.md)
