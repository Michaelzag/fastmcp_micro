# Cache Service (FastMCP)

## Service Name
Cache Service

## Goal
Cache emails and their classification results to provide fast, local access for downstream services.

## MCP Interface
- **Tools:**
  - `add_to_cache()`
    - Signature: `def add_to_cache(self, email: Email, classification: ClassificationResult) -> bool`
    - Description: Add an email and its classification to the cache.
    - Input schema: `{ "email": { ... }, "classification": { ... } }`
    - Output schema: `{ "success": true }`
    - Example call: `{ "email": { "id": "abc123", ... }, "classification": { "category": "work", ... } }`
- **Resources:**
  - `cache://{email_id}`
    - Signature: `def get_cached_email(self, email_id: str) -> CachedEmail`
    - Description: Return the cached email and classification by ID.
    - Output schema: `{ "email": { ... }, "classification": { ... } }`
    - Example URI: `cache://abc123`
- **Prompts:** None

## Inputs
- MCP tool call: `add_to_cache`
- MCP resource fetch: `cache://{email_id}`
- Email and classification objects

## Outputs
- Tool return: Success/failure
- Resource: Cached email and classification
- Logs: Cache insertions, retrievals, errors

## Testing
- Unit: Mock cache backend, test add/retrieve logic
- Integration: Start FastMCP server, call tool/resource via MCP client, verify response
- Contract: Assert output matches defined schema
- Example: Test cache hit/miss, eviction policy

## Isolation
- Runs as standalone FastMCP server
- No direct DB or filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface

## Dependencies
- Python packages: `fastmcp`, `sqlite3` or `redis`
- MCP runtime

## Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

## Security & Compliance
- No sensitive data logged
- Secrets/config stored securely

## Observability
- Logs all cache operations and errors
- Metrics: cache hit/miss rate, latency

## REST API Exposure
- All MCP tools and resources for this service are also exposed as REST endpoints using FastMCP's FastAPI integration.
- REST endpoints follow FastAPI/FastMCP conventions and are documented via OpenAPI (Swagger).
- Example endpoint: `/tools/add_to_cache` (POST), `/resources/cache/{email_id}` (GET)
- See [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration).

## References
- [FastMCP README: Tools](../../fastmcp/README.md#tools)
- [FastMCP server implementation](../../fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](../../fastmcp/tests)
