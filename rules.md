
## Project Goal (R10)
Maintain clean, maintainable, organized codebase. Adherence essential for project success, AI collaboration.

## Core Principles (R11)
- Clean Code: Write readable, understandable, maintainable, extensible code.
- SRP: Each module/class/function has only one reason to change.
- KISS: Favor simple solutions over complex ones.
- YAGNI: Avoid adding features not currently required.
- DRY: Avoid code duplication. Extract common logic.

## Design Patterns & Architecture (R13)
- Repository Pattern for all database interactions. Isolate data access in repository classes.
- Dependency Injection for service dependencies. Never instantiate within business logic.
- Service Layer between API routes and data repositories for business logic.
- Factory Pattern for complex objects or configuration logic.
- Strategy Pattern for interchangeable algorithms or LLM provider logic.
- Adapter Pattern for external systems/APIs integration, isolate integration points.
- CQRS to separate read/write operations where it improves clarity/scalability.
- Select appropriate design patterns prioritizing clarity, maintainability, extensibility.

## Code Style & Formatting (R14)
- Type Hints: Use extensively for clarity and error prevention.
- Docstrings: Clear, concise for all public code. Google Style (Summary, Args, Returns).
- Comments: Favor self-documenting code. Explain why, not what.

## File Length Rules (R15)
Files: Begin considering refactoring at 200 lines. File have a MAXIMUM 250 lines (excluding comments/blanks) for modularity/readability.
Exceptions: None

## Git Workflow (R16)
- Commit early/often. Each commit = small, logical change.
- Clear, descriptive commit messages following Conventional Commits.

## Security Considerations (R19)
- NEVER commit secrets/keys. Manage secrets with environment variables.
- ALWAYS validate/sanitize external inputs.

## LLM Interaction Guidelines (R20)
- Break complex tasks into smaller, testable sub-tasks.
- Provide clear, concise instructions for AI tasks.
- Provide necessary project context for code generation/modification.
- Thoroughly review AI-generated code.
- Iterate on AI output with feedback.

## Refactoring (R21)
- As files exceed 200 lines, refactoring is necessary. Always refactor using proper design patterns.
- Move the refactored files into the deprecated folder.
- After refactoring is complete, make sure the funcitonality is equivalent.
- Folow Code Quality (R33) directions after refactoring. 

## Python Coding Style (R30)
- Type hints extensively.
- Google Style docstrings for public code.
- Self-documenting code over comments. Explain why, not what.
- Lines ≤88 chars (Black default).
- Files ≤250 lines (excluding comments/blanks).
- PEP 8 naming (snake_case vars/funcs, PascalCase classes).
- Group imports (std lib, third-party, local) with blank lines.
- Double quotes for docstrings, single for regular strings.
- F-strings over .format() or concatenation.
- Specific exception types with meaningful messages.

## Python Configuration (R31)
- Use pyproject.toml and uv.lock.
- Init: uv init myproject (--lib for libraries, --bare minimal setup).
- Add deps: uv add <package>
- Remove deps: uv remove <package>
- PREFER Python 3.13+ (downgrade only if needed).
- Venv: uv venv (--python to specify version).
- Sync: uv sync (updates .venv to match lockfile).
- Run: uv run <command> (auto-syncs first).
- Commit lockfile for reproducibility.
- NEVER use pip - ALWAYS use uv.
- Don't commit venvs or cache files.
- Keep configs minimal and relevant.

## Python Packages & Libraries (R32)
- SQLAlchemy/SQLModel for SQLite with Repository pattern.
- Neo4j Python Driver for graph DB interactions.
- FastAPI for API dev with validation and OpenAPI docs.
- Pydantic for data validation, settings, schemas.
- Granian for FastAPI (granian --interface asgi app:app --workers <num_cores>).
- Gradio for simple interfaces (app.mount("/gradio", gr.routes.App.create_app(interface))).

## Code Quality (R33)
- Run these before running code:
  - Ruff for linting
  - Black for formatting
  - Mypy for static type checking (strict)
- NEVER ignore linting errors, type issues, formatting warnings.
- Fix issues rather than suppressing.
- Use # type: ignore only as last resort with explanation.
- Run tools: 1) Ruff --fix 2) Black 3) Mypy
- Pytest for testing when enabled.
- Testing Status: Disabled

## Development Tools (R34)
- UV for dependency management and venvs.
- Use uv run for scripts/tools for correct env.
- Use proper Python module imports.
- Create proper package structure with __init__.py files.

## Design Pattern Implementation (R35)
- Factory: Use class methods or dedicated factory classes.
- DI: Constructor injection and FastAPI dependency system.
- Repository: Isolate DB access in repository classes.
- Strategy: Abstract base classes and DI for algorithm variants.
- Adapter: Dedicated adapter classes for external integration.
- Types: Use proper generic type annotations for patterns.
- Interface Consistency: Maintain clear interfaces.
- Error Handling: Repository/adapter methods handle exceptions.

## ask_perplexity Tool Usage (R50)
The ask_perplexity MCP tool provides external knowledge through Perplexity AI for accurate information.

Use when:
- Not 100% certain about implementation
- Need clarification on technical concepts
- Require best practices info
- Need to validate understanding

Don't:
- Make assumptions when verification possible
- Provide uncertain information
- Implement based on outdated knowledge

Queries should be:
- Specific, focused
- Clearly articulated
- Technical for implementation details

Priority: Always prefer accurate information over guesses, uncertain implementations, outdated recommendations, or incomplete explanations.
