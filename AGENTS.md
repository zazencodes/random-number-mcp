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

The Release Checklist in `README.md` is canonical — follow it, don't work from memory. Notes for agents:

- The version lives in four fields across three files: `pyproject.toml`, `src/random_number_mcp/__init__.py`, and `server.json` (which carries it both at the top level and under `packages[0]`). Miss one and the release ships inconsistent metadata.
- `CHANGELOG.md` is updated as part of the same commit as the version bump.
- Drafting the GitHub Release is a manual UI step. Publishing to PyPI is automatic from there, via the `release: published` trigger in `.github/workflows/publish.yml`. That step is irreversible — a version can never be overwritten or reused on PyPI — so confirm with the maintainer before cutting a release.

## Git Workflow

Commit message style, based on this repo's history:

- Subject line only, no body — imperative mood, capitalized, no trailing period (e.g. `Fix types for mypy`, `Add --fix flag for ruff check`, `Sort imports`).
- No Conventional Commits prefixes (`feat:`, `fix:`), no emoji, no `Co-Authored-By` trailer.
- One logical change per commit; stage only the files that change (avoid `git add -A`).

Do not commit changes on your own. Make the change, report what you did, and wait for explicit confirmation before staging and committing. Pushing to the remote likewise requires explicit confirmation each time.
