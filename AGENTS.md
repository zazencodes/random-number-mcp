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
- Release from `main`, with the branch merged and pushed first. Fetch before assuming local `main` is current — work is sometimes merged upstream via PR, so a local-only merge can leave you diverged.

### Cutting the release

**Always ask the maintainer for explicit confirmation before this step.** Creating the release publishes to PyPI automatically via the `release: published` trigger in `.github/workflows/publish.yml`, and that is irreversible — a version can never be overwritten or reused on PyPI.

The README describes drafting the release in the GitHub UI. An agent can't do that, so use `gh` instead, matching the conventions of every release to date (`v0.1.0` through `v0.1.3`):

- Tag `vX.Y.Z`, title identical to the tag, target `main`, not a draft or prerelease.
- The body is that version's `CHANGELOG.md` section verbatim, with the `## [X.Y.Z] - DATE` header stripped — the `### Added` / `### Changed` subsections only.

```bash
gh release create vX.Y.Z --title "vX.Y.Z" --target main --notes-file <notes>
```

Afterwards, confirm the run succeeded (`gh run watch`) and that the version actually landed on PyPI — a green workflow alone isn't proof.

## Git Workflow

Commit message style, based on this repo's history:

- Subject line only, no body — imperative mood, capitalized, no trailing period (e.g. `Fix types for mypy`, `Add --fix flag for ruff check`, `Sort imports`).
- No Conventional Commits prefixes (`feat:`, `fix:`), no emoji, no `Co-Authored-By` trailer.
- One logical change per commit; stage only the files that change (avoid `git add -A`).

Do not commit changes on your own. Make the change, report what you did, and wait for explicit confirmation before staging and committing. Pushing to the remote likewise requires explicit confirmation each time.
