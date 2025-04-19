# LLM Agent Service (FastMCP)

## Service Name
LLM Agent Service

## Goal
Handle agentic tasks such as summarization, reasoning, and content generation using LLMs, exposing agent tools and results via MCP.

## MCP Interface
- **Tools:**
  - `run_agent_task()`
    - Signature: `def run_agent_task(self, task: str, content: str) -> AgentResult`
    - Description: Perform an agentic LLM task on the given content.
    - Input schema: `{ "task": str, "content": str }`
    - Output schema: `{ "result": str, ... }`
    - Example call: `{ "task": "summarize", "content": "..." }`
- **Resources:**
  - `agent_result://{task_id}`
    - Signature: `def get_agent_result(self, task_id: str) -> AgentResult`
    - Description: Return the result of an agent task by ID.
    - Output schema: `{ "result": str, ... }`
    - Example URI: `agent_result://task123`
- **Prompts:**
  - `agent_prompt()`
    - Signature: `def agent_prompt(self, prompt: str) -> str`
    - Description: Run a custom prompt through the LLM.
    - Output template: `{ "response": str }`

## REST API Exposure
- All MCP tools, resources, and prompts for this service are also exposed as REST endpoints using FastMCP's FastAPI integration.
- REST endpoints follow FastAPI/FastMCP conventions and are documented via OpenAPI (Swagger).
- Example endpoint: `/tools/run_agent_task` (POST), `/resources/agent_result/{task_id}` (GET), `/prompts/agent_prompt` (POST)
- See [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration).

## Inputs
- MCP tool call: `run_agent_task`
- MCP resource fetch: `agent_result://{task_id}`
- MCP prompt: `agent_prompt`
- Task/content from upstream services
- Environment: LLM API key/credentials

## Outputs
- Tool return: Agent result
- Resource: Agent result object
- Prompt output: LLM response
- Logs: Task executions, errors

## Testing
- Unit: Mock LLM API, test agent logic
- Integration: Start FastMCP server, call tool/resource/prompt via MCP client, verify response
- Contract: Assert output matches defined schema
- Example: Test summarization, reasoning, invalid prompt

## Isolation
- Runs as standalone FastMCP server
- No direct DB or filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface

## Dependencies
- Python packages: `fastmcp`, LLM SDK (e.g., `openai`)
- MCP runtime

## Implementation Details

### Packages Used
- `fastmcp`
- `openai` or `pydantic-ai` (LLM integration)
- `pydantic`
- `logfire`

### Implementation Process
1. **Schema Definition:**
   - Use Pydantic for `AgentTask`, `AgentResult`, etc.
2. **LLM Integration:**
   - Integrate with OpenAI or pydantic-ai for LLM tasks.
   - Securely handle API keys.
3. **MCP Interface:**
   - Expose `run_agent_task`, `get_agent_result`, and `agent_prompt` via FastMCP.
4. **Security:**
   - No sensitive data in logs.
   - Store LLM API keys in env/secret manager.
5. **Testing:**
   - Mock LLM API for unit tests.
   - Contract tests for schema compliance.
6. **REST API Exposure:**
   - Expose via FastAPI, document with OpenAPI.
7. **Deployment:**
   - Containerize, use `.env` for secrets.

## Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

## Security & Compliance
- LLM API keys stored securely
- No sensitive data logged

## Observability
- Logs all agent task executions and errors
- Metrics: agent task latency, error rate

## References
- [FastMCP README: Tools](../../fastmcp/README.md#tools)
- [FastMCP server implementation](../../fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](../../fastmcp/tests)
- [fastmcp_spec.md](../fastmcp_spec.md)
