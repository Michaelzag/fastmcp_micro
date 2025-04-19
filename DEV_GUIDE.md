# FastMCP Microservice Development Guide

This guide provides a clear, repeatable process for creating new FastMCP microservice modules. It is tailored for this project, drawing inspiration from your previous `rules.md` but with more flexibility and practical focus.

---

## 1. Project Initialization (with uv)
- Use [uv](https://github.com/astral-sh/uv) for dependency and environment management.
- **Create a new app project in a directory:**
  ```bash
  uv init <service_name>
  cd <service_name>
  ```
  This creates:
  - `pyproject.toml`
  - `README.md`
  - `main.py`
  - `.python-version`
- **Default is an application project.** (Use `--lib` for libraries.)

## 2. Dependency Management
- **Add core dependencies:**
  ```bash
  uv add fastmcp pydantic fastapi logfire
  ```
- **Add service-specific dependencies as needed:**
  - Gmail: `uv add google-api-python-client google-auth httpx`
  - Classification: `uv add scikit-learn openai`
  - Graph: `uv add neo4j`
  - Queue/Notifications: `uv add redis notifiers`
  - Enrichment: `uv add httpx`
- **Dependencies are managed in `pyproject.toml`.**

## 3. Code Organization & Standards
- Use `main.py` as entry point, or create a `src/` for larger services.
- Add business logic, schemas, and FastMCP registration in the project directory or `src/`.
- Use Pydantic for all schemas.
- Add type annotations everywhere.
- Use `logfire` or standard logging for all logs.
- Include a `README.md` with a summary, usage, and API description.

## 4. Implementation Steps
1. **Define schemas** in a dedicated module (e.g., `schemas.py`).
2. **Implement core logic** (e.g., Gmail fetch, classify, etc.) in service modules.
3. **Expose MCP tools/resources** via FastMCP decorators or registry.
4. **Add FastAPI routes** if direct REST endpoints are needed.
5. **Add config loading** (env vars, .env, or secret manager).
6. **Write tests** (unit, integration, contract) in a `tests/` directory.
7. **Document** all endpoints and usage in `README.md`.

## 5. Running & Testing
- **Run your service:**
  ```bash
  uv run main.py
  ```
- **Add and run tests as you grow the project.**

## 6. Dockerization (Recommended)
- Add a `Dockerfile` for containerization.
- Use multi-stage builds if possible.
- Store secrets/config in env vars or Docker secrets.

## 7. Best Practices
- Keep modules small and focused.
- Use dependency injection for testability.
- Follow the [fastmcp_spec.md](./docs/fastmcp_spec.md) for contract and API compliance.
- Prefer clarity and maintainability over cleverness.
- Log errors and important events, but never log secrets.
- Update `pyproject.toml` after adding dependencies.

## 8. References
- [FastMCP Microservice Overview](./docs/fastmcp_microservice_overview.md)
- [FastMCP Spec](./docs/fastmcp_spec.md)
- [rules.md](./rules.md) _(for additional project rules)_

---

This guide should be used as the starting point for all new FastMCP microservice modules. Adapt as needed for service-specific requirements.
