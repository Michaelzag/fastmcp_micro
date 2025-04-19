# Email Classification Service (FastMCP)

## Service Name
Email Classification Service

## Goal
Classify emails using rules, ML, or LLMs and expose classification results as MCP resources for downstream processing.

## MCP Interface
- **Tools:**
  - `classify_email()`
    - Signature: `def classify_email(self, email: Email) -> ClassificationResult`
    - Description: Classify a given email.
    - Input schema: `{ "email": { ... } }`
    - Output schema: `{ "category": str, "score": float, ... }`
    - Example call: `{ "email": { "id": "abc123", ... } }`
- **Resources:**
  - `classification://{email_id}`
    - Signature: `def get_classification(self, email_id: str) -> ClassificationResult`
    - Description: Return the classification result for the given email ID.
    - Output schema: `{ "category": str, "score": float, ... }`
    - Example URI: `classification://abc123`
- **Prompts:** None

## REST API Exposure
- All MCP tools and resources for this service are also exposed as REST endpoints using FastMCP's FastAPI integration.
- REST endpoints follow FastAPI/FastMCP conventions and are documented via OpenAPI (Swagger).
- Example endpoint: `/tools/classify_email` (POST), `/resources/classification/{email_id}` (GET)
- See [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration).

## Inputs
- MCP tool call: `classify_email`
- MCP resource fetch: `classification://{email_id}`
- Email object (from Gmail Retrieval Service)

## Outputs
- Tool return: Classification result
- Resource: Classification result object
- Logs: Classification attempts, errors

## Testing
- Unit: Mock classifier, test logic
- Integration: Start FastMCP server, call tool/resource via MCP client, verify response
- Contract: Assert output matches defined schema
- Example: Test with various email payloads

## Isolation
- Runs as standalone FastMCP server
- No direct DB or filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface

## Dependencies
- Python packages: `fastmcp`, ML/LLM libraries (e.g., `scikit-learn`, `openai`)
- MCP runtime

## Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

## Security & Compliance
- No sensitive data logged
- Model/LLM API keys stored securely

## Observability
- Logs all classification attempts and errors
- Metrics: classification latency, error rate

## Implementation Details

### Packages Used
- `fastmcp`
- `pydantic`
- `scikit-learn` (for basic ML classifiers)
- `openai` or `pydantic-ai` (for LLM-based classification)
- `logfire` (logging)

### Implementation Process
1. **Schema Definition:**
   - Use Pydantic for `Email` and `ClassificationResult` models.
2. **Classifier Logic:**
   - Implement rule-based and ML/LLM classifiers.
   - Use existing logic from `amz_messages/services/classifier.py` as a base.
3. **MCP Interface:**
   - Implement `classify_email` tool and `get_classification` resource as FastMCP endpoints.
4. **Security:**
   - No sensitive data in logs.
   - Store LLM/ML model credentials securely.
5. **Testing:**
   - Unit tests for each classifier type.
   - Contract tests for output schema.
6. **REST API Exposure:**
   - Expose via FastAPI, documented with OpenAPI.
7. **Deployment:**
   - Containerize, use `.env` for secrets.

## References
- [fastmcp_spec.md](../fastmcp_spec.md)
- [amz_messages classifier logic](../../external_projects/amz_messages/src/amz_messages/services/classifier.py)
- [FastMCP README: Tools](../../fastmcp/README.md#tools)
- [FastMCP server implementation](../../fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](../../fastmcp/tests)
