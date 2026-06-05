---
id: MCP
name: MCP Server
description: "Create Model Context Protocol (MCP) servers to expose tools, resources, and prompts to AI clients. Use when: building custom MCP servers in Python or TypeScript, connecting AI clients to external systems, or creating reusable tool integrations. NOT for: MCP client configuration, or non-MCP API integrations."
icon: 🔌
category: dev
created_at: "2026-06-05"
updated_at: "2026-06-05"
homepage: https://modelcontextprotocol.io/
---

# MCP — Model Context Protocol

Build MCP servers to connect AI clients (Claude, ChatGPT, VS Code, Cursor) to your data and tools via a standardized protocol.

## What MCP provides

An MCP server can expose three types of capabilities:

| Primitive | Description | Example |
|---|---|---|
| **Tools** | Functions the LLM can call | `search_database`, `send_email` |
| **Resources** | File-like data the client can read | File contents, API responses |
| **Prompts** | Pre-written templates for users | `summarize-pr`, `review-code` |

## Installation

```bash
# Python
pip install mcp

# TypeScript/Node
npm install @modelcontextprotocol/sdk
```

## Minimal server (Python)

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("my-server")

@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers."""
    return a + b

@mcp.tool()
def get_weather(city: str) -> str:
    """Get current weather for a city."""
    # Real implementation would call a weather API
    return f"The weather in {city} is sunny, 22°C."

if __name__ == "__main__":
    mcp.run()
```

## Minimal server (TypeScript)

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

const server = new McpServer({ name: "my-server", version: "1.0.0" });

server.tool(
  "add",
  { a: z.number(), b: z.number() },
  async ({ a, b }) => ({
    content: [{ type: "text", text: String(a + b) }]
  })
);

const transport = new StdioServerTransport();
await server.connect(transport);
```

## Adding resources (Python)

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("my-server")

@mcp.resource("file://{path}")
def read_file(path: str) -> str:
    """Read a file and return its contents."""
    with open(path) as f:
        return f.read()

@mcp.resource("db://users/{user_id}")
def get_user(user_id: str) -> str:
    """Fetch user data from the database."""
    # Query your database here
    return f'{{"id": "{user_id}", "name": "Alice"}}'
```

## Adding prompts (Python)

```python
from mcp.server.fastmcp import FastMCP
from mcp.types import PromptMessage, TextContent

mcp = FastMCP("my-server")

@mcp.prompt()
def summarize_pr(pr_number: int, repo: str) -> list[PromptMessage]:
    """Prompt to summarize a pull request."""
    return [
        PromptMessage(
            role="user",
            content=TextContent(
                type="text",
                text=f"Summarize pull request #{pr_number} in {repo}. Focus on: what changed, why, and potential risks."
            )
        )
    ]
```

## Connect to Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS):

```json
{
  "mcpServers": {
    "my-server": {
      "command": "python",
      "args": ["/path/to/my_server.py"]
    }
  }
}
```

For a Node.js server:

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["/path/to/my_server.js"]
    }
  }
}
```

## Connect to Claude Code (CLI)

```bash
# Add a local server
claude mcp add my-server -- python /path/to/my_server.py

# Add with environment variables
claude mcp add my-server -e API_KEY=abc123 -- python /path/to/my_server.py

# List connected servers
claude mcp list

# Remove a server
claude mcp remove my-server
```

## Real-world example: database query server

```python
import sqlite3
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("sqlite-server")
DB_PATH = "myapp.db"

@mcp.tool()
def query_database(sql: str) -> list[dict]:
    """Execute a read-only SQL query and return results as a list of records."""
    conn = sqlite3.connect(DB_PATH)
    conn.row_factory = sqlite3.Row
    cursor = conn.execute(sql)
    rows = [dict(row) for row in cursor.fetchall()]
    conn.close()
    return rows

@mcp.tool()
def list_tables() -> list[str]:
    """List all tables in the database."""
    conn = sqlite3.connect(DB_PATH)
    cursor = conn.execute("SELECT name FROM sqlite_master WHERE type='table'")
    tables = [row[0] for row in cursor.fetchall()]
    conn.close()
    return tables

if __name__ == "__main__":
    mcp.run()
```

## Tool design guidelines

- **Name**: lowercase, underscore-separated, verb-first (`get_user`, `search_docs`, `send_message`)
- **Description**: one sentence explaining what it does and when to call it — this is what the LLM reads
- **Parameters**: use type hints; add descriptions for non-obvious parameters
- Keep each tool focused on one operation — compose in the client, not the server

## Transport types

| Transport | Use case |
|---|---|
| `stdio` | Local servers launched as subprocesses (default for most clients) |
| `SSE` | Remote servers over HTTP (for cloud deployments) |

## Notes

- MCP servers are stateless per-session by default; use a database or file if you need persistence.
- The `FastMCP` class (Python) infers tool schemas from type hints and docstrings automatically.
- Any MCP client can use your server without modification — build once, use everywhere.
- Full reference: `https://modelcontextprotocol.io/docs/develop/build-server`
