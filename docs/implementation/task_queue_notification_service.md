# Task Queue & Notification Service (FastMCP)

## Service Name
Task Queue & Notification Service

## Goal
Track user tasks, manage a queue, and trigger notifications based on rules, exposing task management endpoints via MCP.

## MCP Interface
- **Tools:**
  - `create_task()`
    - Signature: `def create_task(self, description: str, due: datetime | None = None) -> Task`
    - Description: Create a new user task.
    - Input schema: `{ "description": str, "due": "ISO8601 datetime (optional)" }`
    - Output schema: `{ "task_id": str, "status": str, ... }`
    - Example call: `{ "description": "Reply to Alice", "due": "2025-04-20T12:00:00Z" }`
  - `complete_task()`
    - Signature: `def complete_task(self, task_id: str) -> bool`
    - Description: Mark a task as completed.
    - Input schema: `{ "task_id": str }`
    - Output schema: `{ "success": true }`
    - Example call: `{ "task_id": "task123" }`
  - `trigger_notification()`
    - Signature: `def trigger_notification(self, task_id: str, channel: str) -> bool`
    - Description: Trigger a notification for a task.
    - Input schema: `{ "task_id": str, "channel": str }`
    - Output schema: `{ "success": true }`
    - Example call: `{ "task_id": "task123", "channel": "email" }`
- **Resources:**
  - `task://{task_id}`
    - Signature: `def get_task(self, task_id: str) -> Task`
    - Description: Return a task by ID.
    - Output schema: `{ "task_id": str, "description": str, "status": str, ... }`
    - Example URI: `task://task123`
- **Prompts:** None

## Inputs
- MCP tool calls: `create_task`, `complete_task`, `trigger_notification`
- MCP resource fetch: `task://{task_id}`
- Task data from upstream services

## Outputs
- Tool return: Task objects, success/failure
- Resource: Task object
- Logs: Task creation/completion, notification triggers, errors

## Testing
- Unit: Mock queue/notification, test logic
- Integration: Start FastMCP server, call tool/resource via MCP client, verify response
- Contract: Assert output matches defined schema
- Example: Test task lifecycle, notification delivery

## Isolation
- Runs as standalone FastMCP server
- No direct DB or filesystem access except for config/secrets
- Docker container recommended
- Only exposes documented MCP interface

## Dependencies
- Python packages: `fastmcp`, queue/notification libs (e.g., `redis`, `smtplib`)
- MCP runtime

## Versioning & Change Management
- Semver; contract changes require major version bump and contract test update

## Security & Compliance
- No sensitive data logged
- Secrets/config stored securely

## Observability
- Logs all task and notification events, errors
- Metrics: task queue length, notification latency, error rate

## REST API Exposure
- All MCP tools and resources for this service are also exposed as REST endpoints using FastMCP's FastAPI integration.
- REST endpoints follow FastAPI/FastMCP conventions and are documented via OpenAPI (Swagger).
- Example endpoint: `/tools/create_task` (POST), `/tools/complete_task` (POST), `/tools/trigger_notification` (POST), `/resources/task/{task_id}` (GET)
- See [FastMCP FastAPI Integration](../../fastmcp/README.md#fastapi-integration).

## References
- [FastMCP README: Tools](../../fastmcp/README.md#tools)
- [FastMCP server implementation](../../fastmcp/src/fastmcp/server/server.py)
- [FastMCP test suite](../../fastmcp/tests)
