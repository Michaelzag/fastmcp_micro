# Orchestrator Service (FastMCP)

## Service Name
Orchestrator Service

## Goal
Coordinate the execution of the agentic workflow by invoking other FastMCP microservices in the correct sequence, handling workflow logic, and exposing orchestration tools via MCP.

## MCP Interface
- **Tools:**
  - `run_pipeline()`
    - Signature: `def run_pipeline(self, since: datetime | None = None) -> dict`
    - Description: Run the full workflow pipeline, starting from email retrieval through to agentic processing.
    - Input schema: `{ "since": "ISO8601 datetime (optional)" }`
    - Output schema: `{ "status": str, "summary": str, ... }`
    - Example call: `{ "since": "2025-04-18T00:00:00Z" }`
  - `get_status()`
    - Signature: `def get_status(self) -> dict`
    - Description: Return the current status of the orchestrator and sub-workflows.
    - Input schema: `{}`
    - Output schema: `{ "running": bool, "last_run": "ISO8601 datetime", ... }`
    - Example call: `{}`
- **Resources:**
  - `pipeline_run://{run_id}`
    - Signature: `def get_pipeline_run(self, run_id: str) -> PipelineRun`
    - Description: Return the results of a specific pipeline run.
    - Output schema: `{ "run_id": str, "status": str, "details": { ... } }`
    - Example URI: `pipeline_run://run123`
- **Prompts:** None

## Inputs
- MCP tool call: `run_pipeline`, `get_status`
- MCP resource fetch: `pipeline_run://{run_id}`
- Workflow triggers (timer, webhook, manual)
- Environment: Service discovery/config

## Outputs
- Tool return: Pipeline run status/result
- Resource: Pipeline run object
- Logs: Workflow executions, errors

## Testing
- Unit: Mock downstream MCP clients, test workflow logic
- Integration: Start FastMCP server, call tool/resource via MCP client, verify response
- Contract: Assert output matches defined schema
- Example: Test full workflow, partial failure, recovery

## Isolation
- Runs as standalone FastMCP server
- No direct DB or filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface
- Calls other services strictly via MCP protocol

## Dependencies
- Python packages: `fastmcp`, MCP client
- MCP runtime
- Downstream MCP endpoints: Gmail Retrieval, Classification, Cache, Graph, Enrichment, Task Queue, LLM Agent

## Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

## Security & Compliance
- No sensitive data logged
- Secrets/config stored securely

## Observability
- Logs all workflow executions, errors, and sub-service calls
- Metrics: pipeline run latency, error rate

## REST API Exposure
- All MCP tools and resources for this service are also exposed as REST endpoints using FastMCP's FastAPI integration.
- REST endpoints follow FastAPI/FastMCP conventions and are documented via OpenAPI (Swagger).
- Example endpoint: `/tools/run_pipeline` (POST), `/tools/get_status` (POST), `/resources/pipeline_run/{run_id}` (GET)
- See [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration).

## References
- [FastMCP README: Tools](../../fastmcp/README.md#tools)
- [FastMCP server implementation](../../fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](../../fastmcp/tests)
