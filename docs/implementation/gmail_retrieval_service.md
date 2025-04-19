# Gmail Retrieval Service (FastMCP)

## Service Name
Gmail Retrieval Service

## Goal
Fetch new emails from Gmail and expose them as MCP resources using FastMCP for downstream agentic workflows.

## MCP Interface
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

## Inputs
- MCP tool call: `fetch_emails` (see above)
- MCP resource fetch: `email://{id}`
- Environment: `GMAIL_OAUTH_TOKEN` (secret)

## Outputs
- Tool return: List of emails (see schema above)
- Resource: Single email object
- Logs: Fetch attempts, errors, email counts

## Testing
- Unit: Mock Gmail API, test fetch logic
- Integration: Start FastMCP server, call tool/resource via MCP client, verify response
- Contract: Assert output matches defined schema
- Example: Test fetch with/without `since` param, test invalid ID

## Isolation
- Runs as standalone FastMCP server (`FastMCP.run`)
- No direct DB or filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface

## Dependencies
- Gmail API
- Python packages: `fastmcp`, `httpx`, `google-auth`
- MCP runtime

## Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

## Security & Compliance
- OAuth token stored securely (env/secret manager)
- No email content logged
- Only exposes minimal metadata in logs

## Observability
- Logs all fetches, errors, and counts
- Metrics: fetch latency, error rate

## REST API Exposure
- All MCP tools and resources for this service are also exposed as REST endpoints using FastMCP's FastAPI integration.
- REST endpoints follow FastAPI/FastMCP conventions and are documented via OpenAPI (Swagger).
- Example endpoint: `/tools/fetch_emails` (POST), `/resources/email/{id}` (GET)
- See [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration).

## References
- [FastMCP README: Tools](../../fastmcp/README.md#tools)
- [FastMCP server implementation](../../fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](../../fastmcp/tests)
