# Content Enrichment Service (FastMCP)

## Service Name
Content Enrichment Service

## Goal
Enrich graph nodes with additional data from web, APIs, or plugins, and expose enriched nodes as MCP resources.

## MCP Interface
- **Tools:**
  - `enrich_content()`
    - Signature: `def enrich_content(self, node: GraphNode) -> EnrichedNode`
    - Description: Enrich a graph node with external data.
    - Input schema: `{ "node": { ... } }`
    - Output schema: `{ ... }`
    - Example call: `{ "node": { "node_id": "abc123", ... } }`
- **Resources:**
  - `enriched://{node_id}`
    - Signature: `def get_enriched_node(self, node_id: str) -> EnrichedNode`
    - Description: Return the enriched node by ID.
    - Output schema: `{ ... }`
    - Example URI: `enriched://abc123`
- **Prompts:** None

## Inputs
- MCP tool call: `enrich_content`
- MCP resource fetch: `enriched://{node_id}`
- Graph node object

## Outputs
- Tool return: Enriched node
- Resource: Enriched node object
- Logs: Enrichment attempts, errors

## Testing
- Unit: Mock enrichment plugins, test logic
- Integration: Start FastMCP server, call tool/resource via MCP client, verify response
- Contract: Assert output matches defined schema
- Example: Test enrichment with various plugins

## Isolation
- Runs as standalone FastMCP server
- No direct DB or filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface

## Dependencies
- Python packages: `fastmcp`, HTTP client, plugin manager
- MCP runtime

## Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

## Security & Compliance
- No sensitive data logged
- Secrets/config stored securely

## Observability
- Logs all enrichment attempts and errors
- Metrics: enrichment latency, error rate

## Implementation Details

### Packages Used
- `fastmcp`
- `httpx` (for async enrichment calls)
- `pydantic`
- `logfire`
- Plugin management: custom or `importlib`

### Implementation Process
1. **Schema Definition:**
   - Use Pydantic for `GraphNode` and `EnrichedNode`.
2. **Plugin System:**
   - Design a plugin interface for enrichment modules.
   - Dynamically load plugins (using `importlib` or similar).
3. **MCP Interface:**
   - Expose `enrich_content` tool and `get_enriched_node` resource.
4. **Security:**
   - Store API keys in env/secret manager.
   - No sensitive data in logs.
5. **Testing:**
   - Unit tests for plugin logic.
   - Contract tests for schemas.
6. **REST API Exposure:**
   - Expose via FastAPI, document with OpenAPI.
7. **Deployment:**
   - Containerize, use `.env` for secrets.

## REST API Exposure
- All MCP tools and resources for this service are also exposed as REST endpoints using FastMCP's FastAPI integration.
- REST endpoints follow FastAPI/FastMCP conventions and are documented via OpenAPI (Swagger).
- Example endpoint: `/tools/enrich_content` (POST), `/resources/enriched/{node_id}` (GET)
- See [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration).

## References
- [FastMCP README: Tools](../../fastmcp/README.md#tools)
- [FastMCP server implementation](../../fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](../../fastmcp/tests)
- [fastmcp_spec.md](../fastmcp_spec.md)
