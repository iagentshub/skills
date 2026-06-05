<div align="center">
  <a href="index.md">← Index</a> &nbsp;·&nbsp;
  <a href="../es/agent-platforms.md">🇪🇸 Ver en Español</a>
</div>

<br>

# Agent Platforms Reference

A comparison of the four major platforms for building and deploying AI agents, and how to move configurations between them.

---

## Overview

| Platform | Primary use | Language | Key primitive |
|---|---|---|---|
| [OpenAI Agents SDK](#openai-agents-sdk) | Orchestrated Python agents | Python | `Agent` + `Runner` |
| [Anthropic API](#anthropic-api) | Claude agents with tool use | Python, TypeScript | `messages.create` + tools |
| [MCP](#mcp-model-context-protocol) | Reusable tool servers for any client | Python, TypeScript | `@mcp.tool()` |
| [GitHub Copilot Skills](#github-copilot-skills) | IDE/workflow automation for Copilot | Markdown + shell | `SKILL.md` |

---

## OpenAI Agents SDK

**When to choose:** You want a high-level Python framework with built-in multi-agent orchestration, handoffs, guardrails, and sessions.

**Core concepts:**
- `Agent(name, instructions, tools, handoffs, output_type)` — defines an agent
- `@function_tool` — wraps a Python function as an agent-callable tool
- `Runner.run_sync()` / `Runner.run()` — executes the agent loop
- `handoff(agent)` — delegates conversation to a specialist agent
- `agent.as_tool()` — exposes an agent as a callable tool (manager pattern)

**Skill:** [openai-agents](../public/en/openai-agents/SKILL.md)
**Docs:** https://openai.github.io/openai-agents-python/

---

## Anthropic API

**When to choose:** You need direct control over the Claude API, custom agentic loops, server-side tools (web_search, code_execution), or TypeScript support.

**Core concepts:**
- Tools defined as JSON schemas with `name`, `description`, `input_schema`
- `stop_reason: "tool_use"` signals Claude wants to call a tool
- `tool_result` blocks send execution results back to Claude
- `tool_choice` controls when tools are called (`auto`, `any`, `tool`, `none`)
- `strict: true` guarantees schema conformance

**Tool types:**

| Type | Examples | Executed by |
|---|---|---|
| Client tools | Custom functions, DB queries | Your code |
| Server tools | `web_search`, `code_execution`, `web_fetch` | Anthropic |

**Skill:** [anthropic-api](../public/en/anthropic-api/SKILL.md)
**Docs:** https://docs.anthropic.com/

---

## MCP (Model Context Protocol)

**When to choose:** You want to build a reusable integration that works with any MCP-compatible client (Claude, ChatGPT, VS Code, Cursor) without rewriting it per platform.

**Core concepts:**
- **Tools** — callable functions (`@mcp.tool()`)
- **Resources** — readable data (`@mcp.resource("scheme://{param}")`)
- **Prompts** — reusable prompt templates (`@mcp.prompt()`)
- **Transport** — `stdio` (local) or `SSE` (remote/cloud)

**Client configuration:**

```json
// Claude Desktop / Claude Code
{ "mcpServers": { "name": { "command": "python", "args": ["server.py"] } } }
```

**Skill:** [mcp](../public/en/mcp/SKILL.md)
**Docs:** https://modelcontextprotocol.io/

---

## GitHub Copilot Skills

**When to choose:** You want to extend GitHub Copilot Cloud Agent with custom instructions, scripts, or workflows — tied to a repository or user profile.

**Core concepts:**
- Skills = `SKILL.md` + optional scripts in a dedicated folder
- Copilot loads skills automatically when the prompt matches the description
- `allowed-tools: [shell]` pre-approves shell execution
- Managed with `gh skill` CLI (search, install, update, publish)

**Scope:**

| Location | Scope |
|---|---|
| `.github/skills/` | Shared with team, version-controlled |
| `~/.copilot/skills/` | Personal only |

**Skill:** [github-copilot-skills](../public/en/github-copilot-skills/SKILL.md)
**Docs:** https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-skills

---

## Choosing the right platform

```
Need a reusable tool server any AI client can use?
  → MCP

Building with Claude directly (Anthropic)?
  → anthropic-api

Building a Python multi-agent system (OpenAI)?
  → openai-agents

Extending GitHub Copilot with custom workflows?
  → github-copilot-skills
```

---

## Moving configurations between platforms

The hub architecture allows sharing agent logic across platforms. Common patterns:

### Tool → MCP server
A tool defined in Python for one platform can become an MCP server, making it available to all MCP clients:

```python
# Original: Anthropic client tool
def search_docs(query: str) -> list[dict]: ...

# As MCP server — no logic changes needed
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("docs-server")

@mcp.tool()
def search_docs(query: str) -> list[dict]:
    """Search internal documentation."""
    ...  # same implementation
```

### Agent instructions → Copilot skill
The description and instructions from any agent can be packaged as a Copilot skill:

```markdown
---
name: your-agent-name
description: "What the agent does and when Copilot should activate it."
---

# Agent Name

[Agent instructions here]
```

### OpenAI tool → Anthropic tool
Both platforms use similar JSON schemas; only the wrapper differs:

```python
# OpenAI Agents SDK
@function_tool
def get_user(user_id: str) -> dict: ...

# Anthropic API equivalent
{
  "name": "get_user",
  "description": "Fetch a user record by ID.",
  "input_schema": {
    "type": "object",
    "properties": {"user_id": {"type": "string"}},
    "required": ["user_id"]
  }
}
```
