# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.2] - 2026-03-03

### Added
- MCP Registry metadata via `server.json`, including package and transport details.
- MCP Registry identifier (`mcp-name`) in `README.md`.

### Changed
- Disabled the FastMCP startup banner in `src/random_number_mcp/server.py` for cleaner STDIO output.
- Documentation cleanup in `README.md`.

## [0.1.1] - 2025-06-29

### Added
- `random_sample` tool to choose k unique items from a population.
- Support for JSON string input for `weights` in the `random_choices` tool.
- Comprehensive test suite for `random_sample`.
- Tests for `random_choices` with JSON string weights.
- Demo video and MCP server badge in `README.md`.
- MCP client configuration example in `README.md`.
- Detailed release checklist for developers in `README.md`.

### Changed
- Updated project description in `README.md`.

## [0.1.0] - 2025-06-25

### Added
- Initial release of random-number-mcp
- FastMCP v2.0 server implementation
- Six random number utility tools:
  - `random_int` - Generate random integers using `random.randint()`
  - `random_float` - Generate random floats using `random.uniform()`
  - `random_choices` - Choose items from a list using `random.choices()` with optional weights
  - `random_shuffle` - Shuffle items into a new list using `random.sample()`
  - `secure_token_hex` - Generate cryptographically secure hex tokens using `secrets.token_hex()`
  - `secure_random_int` - Generate cryptographically secure integers using `secrets.randbelow()`
- Comprehensive input validation and error handling
- Type hints throughout the codebase
- Complete test suite with pytest
- Documentation and examples
- PyPI packaging with uv build system
- MIT License

### Features
- JSON-RPC 2.0 transport over STDIO
- Support for Python 3.10+
- Extensible architecture for adding new tools
- Both standard and cryptographically secure random functions
- Comprehensive error handling with descriptive messages
- Full MCP protocol compliance

### Developer Experience
- Modern Python packaging with pyproject.toml
- Code quality tools (ruff, mypy)
- Comprehensive test coverage
- Developer documentation
- GitHub Actions for CI/CD
