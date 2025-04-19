# FastMCP Service Specification (Technical, Ironclad)

This document defines the canonical, technical specification format for designing and documenting FastMCP-based services. The spec is tailored for the FastMCP framework and should be used as the single source of truth for all MCP service definitions in this project. It is intended to be both human- and LLM-readable, and to provide enough detail for robust, automated reasoning and implementation.

---

## Specification Fields

### 1. **Service Name**
- Unique, descriptive name for the FastMCP service.

### 2. **Goal**
- One-sentence summary of the service's single responsibility, referencing the MCP paradigm (see [FastMCP README](./fastmcp/README.md)).

### 3. **MCP Interface**
- **Tools:** List of MCP tools exposed (see [`@mcp.tool()`](./fastmcp/README.md#tools) and [`FastMCP.tool`](./fastmcp/src/fastmcp/server/server.py)).
  - For each tool, specify:
    - Name (as exposed to MCP clients)
    - Python signature (with type hints)
    - Description/docstring
    - Expected input schema (types, validation)
    - Expected output schema (types, validation)
    - Example MCP call payload
- **Resources:** List of MCP resources (see [`@mcp.resource()`](./fastmcp/README.md#resources) and [`FastMCP.resource`](./fastmcp/src/fastmcp/server/server.py)).
  - For each resource, specify:
    - URI pattern (e.g., `email://{id}`)
    - Python signature
    - Description/docstring
    - Output schema
    - Example MCP resource fetch
- **Prompts:** List of MCP prompts (see [`@mcp.prompt()`](./fastmcp/README.md#prompts)).
  - For each prompt, specify:
    - Name
    - Python signature
    - Description
    - Output template/schema

### 4. **Inputs**
- List all data, triggers, and events that enter the service, including:
  - MCP tool calls (with example payloads)
  - Resource fetches (with example URIs)
  - Prompts (with example arguments)
  - External triggers (timers, webhooks, etc.)
  - Required environment variables/secrets

### 5. **Outputs**
- List all data, events, and side effects produced by the service, including:
  - MCP tool return values (with example payloads)
  - MCP resources (with example data)
  - Prompts (output format)
  - External effects (notifications, DB writes, etc.)

### 6. **Testing**
- **Unit tests:** List logic and edge cases to be tested.
- **Integration tests:** Specify how to test MCP interface compliance (see [tests/](./fastmcp/tests)).
- **Contract tests:** Define input/output schemas and contract assertions for MCP calls (see [FastMCP schema generation](./fastmcp/README.md#tools)).
- **Example test scenarios:** Provide at least one for each interface.

### 7. **Isolation**
- Explicitly state:
  - The service runs as a standalone FastMCP server (see [`FastMCP.run`](./fastmcp/src/fastmcp/server/server.py)).
  - No direct access to other services' code, DB, or filesystem (except via MCP or explicitly defined roots).
  - Docker containerization is recommended for process isolation.
  - Network, file, and environment boundaries.
  - Only allowed to communicate via MCP or explicitly documented protocols.

### 8. **Dependencies**
- List all runtime dependencies, including:
  - External APIs (e.g., Gmail, LLM provider)
  - Required infrastructure (database, cache, queue, etc.)
  - MCP endpoints it expects to call (with interface references)
  - Python packages (with version constraints, if relevant)

### 9. **Versioning & Change Management**
- Versioning scheme (e.g., semver)
- Policy for breaking changes
- Reference to contract test updates

### 10. **Security & Compliance**
- Data access restrictions
- Authentication/authorization requirements (see [FastMCP settings](./fastmcp/src/fastmcp/settings.py))
- Handling of secrets and sensitive data
- Logging and privacy policy

### 11. **Observability**
- Logging requirements (see [FastMCP logging](./fastmcp/src/fastmcp/server/context.py))
- Metrics/tracing (latency, error rates, etc.)
- Alerting and monitoring hooks

### 12. **References**
- List relevant FastMCP source files, documentation sections, and test cases that define or illustrate key behaviors for this service.

### 13. **REST API Exposure Requirement**
- **Requirement:** Every MCP tool, resource, and prompt defined in this spec MUST also be exposed as a REST endpoint using FastMCP's FastAPI integration.
- **Implementation:**
  - Each MCP tool/resource/prompt should be accessible via an auto-generated or explicitly defined REST path.
  - REST endpoints MUST follow FastAPI and FastMCP conventions for path naming, input/output schema, and error handling.
  - REST endpoints SHOULD be documented via OpenAPI (Swagger) auto-generated docs.
- **Purpose:**
  - Enables interoperability with HTTP clients, webhooks, and non-MCP systems.
  - Simplifies integration testing, debugging, and admin access.
- **References:**
  - [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration)
  - [FastAPI Docs](https://fastapi.tiangolo.com/)
  - [OpenAPI/Swagger](https://swagger.io/specification/)

---

## Example (Technical, FastMCP-Specific)

### Service Name
**Gmail Retrieval Service**

### Goal
Fetch new emails from Gmail and expose them as MCP resources using FastMCP for downstream agentic workflows.

### MCP Interface
- **Tools:**
  - `fetch_emails()`
    - Signature: `def fetch_emails(self, since: datetime | None = None) -> list[Email]`
    - Description: Fetch new emails since the given datetime.
    - Input schema: `{ "since": "ISO8601 datetime (optional)" }`
    - Output schema: `[ { "id": str, "from": str, "subject": str, ... } ]`
    - Example call: `{ "since": "2025-04-18T00:00:00Z" }`
- **Resources:**
  - `email://{id}`
    - Signature: `def get_email(self, id: str) -> Email`
    - Description: Return the email with the given ID.
    - Output schema: `{ "id": str, "from": str, "subject": str, ... }`
    - Example URI: `email://abc123`
- **Prompts:** None

### Inputs
- MCP tool call: `fetch_emails` (see above)
- MCP resource fetch: `email://{id}`
- Environment: `GMAIL_OAUTH_TOKEN` (secret)

### Outputs
- Tool return: List of emails (see schema above)
- Resource: Single email object
- Logs: Fetch attempts, errors, email counts

### Testing
- Unit: Mock Gmail API, test fetch logic
- Integration: Start FastMCP server, call tool/resource via MCP client, verify response
- Contract: Assert output matches defined schema
- Example: Test fetch with/without `since` param, test invalid ID

### Isolation
- Runs as standalone FastMCP server (`FastMCP.run`)
- No direct DB or filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface

### Dependencies
- Gmail API
- Python packages: `fastmcp`, `httpx`, `google-auth`
- MCP runtime

### Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

### Security & Compliance
- OAuth token stored securely (env/secret manager)
- No email content logged
- Only exposes minimal metadata in logs

### Observability
- Logs all fetches, errors, and counts
- Metrics: fetch latency, error rate

### References
- [FastMCP README: Tools](./fastmcp/README.md#tools)
- [FastMCP server implementation](./fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](./fastmcp/tests)

---

**Use this template for every FastMCP service. Reference relevant source and documentation sections to ensure clarity for both humans and LLMs.**
