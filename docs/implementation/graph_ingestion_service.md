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

## Implementation Details

### Packages Used
- `fastmcp`
- `neo4j` (official Python driver)
- `pydantic`
- `logfire`

### Implementation Process
1. **Schema Definition:**
   - Define `GraphNode` and related schemas with Pydantic.
2. **Neo4j Integration:**
   - Use `neo4j` driver for all DB operations.
   - Implement connection pooling and error handling.
3. **MCP Interface:**
   - Expose `add_to_graph`, `query_graph` tools, and `get_graph_node` resource.
4. **Security:**
   - Store DB credentials in env/secret manager.
   - No sensitive data in logs.
5. **Testing:**
   - Mock Neo4j for unit tests.
   - Contract tests for schema compliance.
6. **REST API Exposure:**
   - Expose via FastAPI, document with OpenAPI.
7. **Deployment:**
   - Containerize, use `.env` for secrets.

## REST API Exposure
- All MCP tools and resources for this service are also exposed as REST endpoints using FastMCP's FastAPI integration.
- REST endpoints follow FastAPI/FastMCP conventions and are documented via OpenAPI (Swagger).
- Example endpoint: `/tools/add_to_graph` (POST), `/tools/query_graph` (POST), `/resources/graph/{node_id}` (GET)
- See [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration).

## References
- [fastmcp_spec.md](../fastmcp_spec.md)
- [amz_messages parsing/graph logic](../../external_projects/amz_messages/src/amz_messages/parsers/)
- [FastMCP README: Tools](../../fastmcp/README.md#tools)
- [FastMCP server implementation](../../fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](../../fastmcp/tests)
