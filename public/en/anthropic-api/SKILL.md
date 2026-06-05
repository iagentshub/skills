---
id: ANTHROPIC_API
name: Anthropic API
description: "Build agents and tools with the Anthropic Claude API (Python/TypeScript). Use when: defining client tools, implementing agentic loops, using computer use, building with tool_use, or integrating Claude into applications. NOT for: Gemini, OpenAI, or MCP server creation."
icon: 🧠
category: ai
created_at: "2026-06-05"
updated_at: "2026-06-05"
homepage: https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/overview
---

# Anthropic API — Agents & Tool Use

Use the Anthropic SDK to build agents with tool calling, agentic loops, and structured outputs.

## Installation

```bash
pip install anthropic          # Python
npm install @anthropic-ai/sdk  # TypeScript/Node
```

Requires `ANTHROPIC_API_KEY` in the environment.

## How tool use works

1. You define tools (name, description, input schema).
2. Claude decides whether to call a tool based on the user's request.
3. If Claude calls a tool (`stop_reason: "tool_use"`), your code executes it.
4. You send back the result as `tool_result`.
5. Claude uses the result to continue.

**Client tools** — your code executes them (database queries, file ops, external APIs).
**Server tools** — Anthropic executes them directly (web_search, code_execution, web_fetch).

## Basic example (Python)

```python
import anthropic

client = anthropic.Anthropic()

tools = [
    {
        "name": "get_weather",
        "description": "Get current weather for a location.",
        "input_schema": {
            "type": "object",
            "properties": {
                "location": {"type": "string", "description": "City name"}
            },
            "required": ["location"]
        }
    }
]

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": "What's the weather in Paris?"}],
)
print(response.content)
```

## Agentic loop (Python)

```python
import anthropic
import json

client = anthropic.Anthropic()

def get_weather(location: str) -> str:
    return f"The weather in {location} is 18°C and cloudy."

def run_agent(user_message: str):
    messages = [{"role": "user", "content": user_message}]
    tools = [
        {
            "name": "get_weather",
            "description": "Get current weather for a location.",
            "input_schema": {
                "type": "object",
                "properties": {
                    "location": {"type": "string"}
                },
                "required": ["location"]
            }
        }
    ]

    while True:
        response = client.messages.create(
            model="claude-opus-4-8",
            max_tokens=1024,
            tools=tools,
            messages=messages,
        )

        if response.stop_reason == "end_turn":
            return next(b.text for b in response.content if b.type == "text")

        # Process tool calls
        tool_results = []
        for block in response.content:
            if block.type == "tool_use":
                if block.name == "get_weather":
                    result = get_weather(**block.input)
                    tool_results.append({
                        "type": "tool_result",
                        "tool_use_id": block.id,
                        "content": result,
                    })

        messages.append({"role": "assistant", "content": response.content})
        messages.append({"role": "user", "content": tool_results})

result = run_agent("What's the weather in Madrid and London?")
print(result)
```

## Server tools (no execution code needed)

```python
# web_search — Anthropic executes it
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    tools=[{"type": "web_search_20260209", "name": "web_search"}],
    messages=[{"role": "user", "content": "Latest news on AI agents"}],
)

# code_execution — runs Python in a sandbox
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=4096,
    tools=[{"type": "code_execution_20250522", "name": "code_execution"}],
    messages=[{"role": "user", "content": "Calculate the first 10 Fibonacci numbers"}],
)
```

## Structured output (JSON)

```python
tools = [
    {
        "name": "extract_person",
        "description": "Extract person data from text.",
        "input_schema": {
            "type": "object",
            "properties": {
                "name": {"type": "string"},
                "age": {"type": "integer"},
                "email": {"type": "string"}
            },
            "required": ["name"]
        }
    }
]

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=512,
    tools=tools,
    tool_choice={"type": "tool", "name": "extract_person"},  # Force this tool
    messages=[{"role": "user", "content": "John Doe, 32, john@example.com"}],
)

# The tool input IS the structured data
tool_call = next(b for b in response.content if b.type == "tool_use")
person = tool_call.input  # {"name": "John Doe", "age": 32, "email": "john@example.com"}
```

## Strict tool use

```python
tools = [
    {
        "name": "my_tool",
        "description": "...",
        "input_schema": {...},
        "strict": True  # Claude's calls always match the schema exactly
    }
]
```

## Forcing and controlling tool use

```python
# Force a specific tool
tool_choice={"type": "tool", "name": "get_weather"}

# Require at least one tool
tool_choice={"type": "any"}

# Let Claude decide (default)
tool_choice={"type": "auto"}

# Never call tools
tool_choice={"type": "none"}
```

## TypeScript example

```typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();

const response = await client.messages.create({
  model: "claude-opus-4-8",
  max_tokens: 1024,
  tools: [
    {
      name: "get_weather",
      description: "Get current weather for a location.",
      input_schema: {
        type: "object",
        properties: {
          location: { type: "string" }
        },
        required: ["location"]
      }
    }
  ],
  messages: [{ role: "user", content: "Weather in Tokyo?" }]
});
```

## Current models

| Model | ID | Best for |
|---|---|---|
| Claude Opus 4.8 | `claude-opus-4-8` | Complex reasoning, agents |
| Claude Sonnet 4.6 | `claude-sonnet-4-6` | Balanced speed/capability |
| Claude Haiku 4.5 | `claude-haiku-4-5-20251001` | Fast, high-volume tasks |

## Notes

- Always check `stop_reason` before reading `content` — it's `"tool_use"` when tools are called, `"end_turn"` when done.
- Tool descriptions are the most important field — Claude's decision to call a tool depends almost entirely on the description.
- For agentic loops, append both the assistant's response and tool results to `messages` before the next API call.
- Full reference: `https://docs.anthropic.com/`
