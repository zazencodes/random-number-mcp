# Random Number MCP

Essential random number generation utilities from the Python standard library, including pseudorandom and cryptographically secure operations for integers, floats, weighted selections, list shuffling, and secure token generation.

## Demo Video

https://github.com/user-attachments/assets/303a441a-2b10-47e3-b2a5-c8b51840e362

<a href="https://glama.ai/mcp/servers/@zazencodes/random-number-mcp">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/@zazencodes/random-number-mcp/badge" alt="Random Number MCP server" />
</a>

## Tools


| Tool                | Purpose                                      | Python function   |
| ------------------- | -------------------------------------------- | --------------------- |
| `random_int`        | Generate random integers                     | `random.randint()`    |
| `random_float`      | Generate random floats                       | `random.uniform()`    |
| `random_choices`    | Choose items from a list (optional weights)  | `random.choices()`    |
| `random_shuffle`    | Return a new list with items shuffled        | `random.sample()`     |
| `random_sample`     | Choose k unique items from population        | `random.sample()`     |
| `secure_token_hex`  | Generate cryptographically secure hex tokens | `secrets.token_hex()` |
| `secure_random_int` | Generate cryptographically secure integers   | `secrets.randbelow()` |


## Setup

### Claude Desktop

Add this to your Claude Desktop configuration file:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`   
**Windows**: `%APPDATA%/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "random-number": {
      "command": "uvx",
      "args": ["random-number-mcp"]
    }
  }
}
```

## Tool Reference

### `random_int`

Generate a random integer between low and high (inclusive).

**Parameters:**
- `low` (int): Lower bound (inclusive)
- `high` (int): Upper bound (inclusive)

**Example:**
```json
{
  "name": "random_int",
  "arguments": {
    "low": 1,
    "high": 100
  }
}
```

### `random_float`

Generate a random float between low and high.

**Parameters:**
- `low` (float, optional): Lower bound (default: 0.0)
- `high` (float, optional): Upper bound (default: 1.0)

**Example:**
```json
{
  "name": "random_float",
  "arguments": {
    "low": 0.5,
    "high": 2.5
  }
}
```

### `random_choices`

Choose k items from a population with replacement, optionally weighted.

**Parameters:**
- `population` (list): List of items to choose from
- `k` (int, optional): Number of items to choose (default: 1)
- `weights` (list, optional): Weights for each item (default: equal weights)

**Example:**
```json
{
  "name": "random_choices",
  "arguments": {
    "population": ["red", "blue", "green", "yellow"],
    "k": 2,
    "weights": [0.4, 0.3, 0.2, 0.1]
  }
}
```

### `random_shuffle`

Return a new list with items in random order.

**Parameters:**
- `items` (list): List of items to shuffle

**Example:**
```json
{
  "name": "random_shuffle",
  "arguments": {
    "items": [1, 2, 3, 4, 5]
  }
}
```

### `random_sample`

Choose k unique items from population without replacement.

**Parameters:**
- `population` (list): List of items to choose from
- `k` (int): Number of items to choose

**Example:**
```json
{
  "name": "random_sample",
  "arguments": {
    "population": ["a", "b", "c", "d", "e"],
    "k": 2
  }
}
```

### `secure_token_hex`

Generate a cryptographically secure random hex token.

**Parameters:**
- `nbytes` (int, optional): Number of random bytes (default: 32)

**Example:**
```json
{
  "name": "secure_token_hex",
  "arguments": {
    "nbytes": 16
  }
}
```

### `secure_random_int`

Generate a cryptographically secure random integer below upper_bound.

**Parameters:**
- `upper_bound` (int): Upper bound (exclusive)

**Example:**
```json
{
  "name": "secure_random_int",
  "arguments": {
    "upper_bound": 1000
  }
}
```

## Security Considerations

This package provides both standard pseudorandom functions (suitable for simulations, games, etc.) and cryptographically secure functions (suitable for tokens, keys, etc.):

- **Standard functions** (`random_int`, `random_float`, `random_choices`, `random_shuffle`): Use Python's `random` module - fast but not cryptographically secure
- **Secure functions** (`secure_token_hex`, `secure_random_int`): Use Python's `secrets` module - slower but cryptographically secure

## Development

### Prerequisites

- Python 3.10+
- [uv](https://docs.astral.sh/uv/) package manager

### Setup

```bash
# Clone the repository
git clone https://github.com/example/random-number-mcp
cd random-number-mcp

# Install dependencies
uv sync --dev

# Run tests
uv run pytest

# Run linting
uv run ruff check --fix
uv run ruff format

# Type checking
uv run mypy src/
```

### MCP Client Config

```json
{
  "mcpServers": {
    "random-number-dev": {
      "command": "uv",
      "args": [
        "--directory",
        "<path_to_your_repo>/random-number-mcp",
        "run",
        "random-number-mcp"
      ]
    }
  }
}
```

**Note:** Replace `<path_to_your_repo>/random-number-mcp` with the absolute path to your cloned repository.

### Building

```bash
# Build package
uv build

# Test installation
uv run --with dist/*.whl random-number-mcp
```

### Release Checklist

1.  **Update Version:**
    - Increment the `version` number in `pyproject.toml`, `src/random_number_mcp/__init__.py`, and `server.json`.

2.  **Update Changelog:**
    - Add a new entry in `CHANGELOG.md` for the release.
        - Draft notes with coding agent using `git diff` context.

        ```
        Update the @CHANGELOG.md for the latest release.
        List all significant changes, bug fixes, and new features.
        Here's the git diff:
        [GIT_DIFF]
        ```
        
    - Commit along with any other pending changes.

3.  **Create GitHub Release:**
    - Draft a new release on the GitHub UI.
        - Tag release using UI.
    - The GitHub workflow will automatically build and publish the package to PyPI.

## Testing with MCP Inspector

For exploring and/or developing this server, use the MCP Inspector npm utility:

```bash
# Install MCP Inspector
npm install -g @modelcontextprotocol/inspector

# Run local development server with the inspector
npx @modelcontextprotocol/inspector uv run random-number-mcp

# Run PyPI production server with the inspector
npx @modelcontextprotocol/inspector uvx random-number-mcp
```

## MCP Registry

mcp-name: io.github.zazencodes/random-number-mcp

## License

MIT License - see [LICENSE](LICENSE) file for details.
