# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies
uv sync --dev

# Run tests
uv run pytest

# Run a single test
uv run pytest tests/test_tools.py::test_function_name

# Lint and format
uv run ruff check --fix
uv run ruff format

# Type checking
uv run mypy src/

# Run server locally
uv run random-number-mcp

# Build package
uv build

# Inspect/test with MCP Inspector
npx @modelcontextprotocol/inspector uv run random-number-mcp
```

## Architecture

The package is a [FastMCP](https://github.com/jlowin/fastmcp) server with a three-layer structure:

- **`server.py`** — FastMCP app instance and `@app.tool()` decorated endpoints. Handles MCP protocol concerns (e.g., JSON string weights parsing). Entry point is `main()`.
- **`tools.py`** — Pure Python business logic, calls `random` and `secrets` stdlib modules. No FastMCP dependency; testable in isolation.
- **`utils.py`** — Shared validation helpers used by `tools.py`.

Tools fall into two categories: standard pseudorandom (`random` module) and cryptographically secure (`secrets` module).

## Release Process

Version must be updated in three places: `pyproject.toml`, `src/random_number_mcp/__init__.py`, and `server.json`. Publishing to PyPI is triggered automatically by creating a GitHub Release with a tag.
