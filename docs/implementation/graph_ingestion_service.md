# Graph Database Ingestion Service (FastMCP)

## Service Name
Graph Database Ingestion Service

## Goal
Ingest classified emails into a graph database, mapping relationships and exposing graph nodes/edges as MCP resources.

## MCP Interface
- **Tools:**
  - `add_to_graph()`
    - Signature: `def add_to_graph(self, email: Email, classification: ClassificationResult) -> GraphNode`
    - Description: Add a classified email to the graph database.
    - Input schema: `{ "email": { ... }, "classification": { ... } }`
    - Output schema: `{ "node_id": str, ... }`
    - Example call: `{ "email": { "id": "abc123", ... }, "classification": { ... } }`
  - `query_graph()`
    - Signature: `def query_graph(self, query: str) -> list[GraphNode]`
    - Description: Query the graph database for nodes/edges.
    - Input schema: `{ "query": str }`
    - Output schema: `[ { ... } ]`
    - Example call: `{ "query": "MATCH (e:Email) RETURN e" }`
- **Resources:**
  - `graph://{node_id}`
    - Signature: `def get_graph_node(self, node_id: str) -> GraphNode`
    - Description: Return the graph node by ID.
    - Output schema: `{ ... }`
    - Example URI: `graph://abc123`
- **Prompts:** None

## Inputs
- MCP tool calls: `add_to_graph`, `query_graph`
- MCP resource fetch: `graph://{node_id}`
- Email and classification objects

## Outputs
- Tool return: Graph node(s)
- Resource: Graph node object
- Logs: Ingestion attempts, queries, errors

## Testing
- Unit: Mock graph DB, test add/query logic
- Integration: Start FastMCP server, call tool/resource via MCP client, verify response
- Contract: Assert output matches defined schema
- Example: Test various graph queries

## Isolation
- Runs as standalone FastMCP server
- No direct DB or filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface

## Dependencies
- Python packages: `fastmcp`, graph DB client (e.g., `neo4j`)
- MCP runtime

## Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

## Security & Compliance
- No sensitive data logged
- DB credentials stored securely

## Observability
- Logs all ingestion/query attempts and errors
- Metrics: ingestion/query latency, error rate

## REST API Exposure
- All MCP tools and resources for this service are also exposed as REST endpoints using FastMCP's FastAPI integration.
- REST endpoints follow FastAPI/FastMCP conventions and are documented via OpenAPI (Swagger).
- Example endpoint: `/tools/add_to_graph` (POST), `/tools/query_graph` (POST), `/resources/graph/{node_id}` (GET)
- See [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration).

## References
- [FastMCP README: Tools](../../fastmcp/README.md#tools)
- [FastMCP server implementation](../../fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](../../fastmcp/tests)
