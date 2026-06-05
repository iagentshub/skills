---
id: OPENAI_AGENTS
name: OpenAI Agents SDK
description: "Build and orchestrate AI agents with the OpenAI Agents SDK (Python). Use when: creating agents with tools, multi-agent handoffs, guardrails, structured outputs, or persistent sessions. NOT for: direct API calls without agents, OpenAI Whisper/TTS, or non-Python stacks."
icon: 🤖
category: ai
created_at: "2026-06-05"
updated_at: "2026-06-05"
homepage: https://openai.github.io/openai-agents-python/
---

# OpenAI Agents SDK

Use the OpenAI Agents SDK to build, run, and orchestrate AI agents in Python. Three core primitives: **Agents**, **Handoffs**, and **Guardrails**.

## Installation

```bash
pip install openai-agents
```

Requires `OPENAI_API_KEY` set in the environment.

## Basic agent

```python
from agents import Agent, Runner

agent = Agent(
    name="Assistant",
    instructions="You are a helpful assistant.",
    model="gpt-4o",
)

result = Runner.run_sync(agent, "What is the capital of France?")
print(result.final_output)
```

## Agent with tools

```python
from agents import Agent, Runner, function_tool

@function_tool
def get_weather(city: str) -> str:
    """Returns current weather for the given city."""
    return f"The weather in {city} is sunny, 22°C."

agent = Agent(
    name="Weather Agent",
    instructions="Help users with weather questions. Use get_weather when asked.",
    tools=[get_weather],
)

result = Runner.run_sync(agent, "What's the weather in Madrid?")
print(result.final_output)
```

## Structured output

```python
from pydantic import BaseModel
from agents import Agent, Runner

class Summary(BaseModel):
    title: str
    key_points: list[str]

agent = Agent(
    name="Summarizer",
    instructions="Summarize the user's text.",
    output_type=Summary,
)

result = Runner.run_sync(agent, "Explain quantum computing in 3 key points.")
summary: Summary = result.final_output
print(summary.title, summary.key_points)
```

## Multi-agent: handoffs

```python
from agents import Agent, Runner, handoff

spanish_agent = Agent(
    name="Spanish Agent",
    instructions="You only respond in Spanish.",
)

triage_agent = Agent(
    name="Triage",
    instructions="Route to the Spanish agent for Spanish speakers.",
    handoffs=[handoff(spanish_agent)],
)

result = Runner.run_sync(triage_agent, "Hola, necesito ayuda.")
print(result.final_output)
```

## Multi-agent: manager pattern

```python
from agents import Agent, Runner

researcher = Agent(name="Researcher", instructions="Research facts.")
writer = Agent(name="Writer", instructions="Write based on research.")

# Manager uses sub-agents as tools via as_tool()
manager = Agent(
    name="Manager",
    instructions="Coordinate research and writing.",
    tools=[researcher.as_tool(), writer.as_tool()],
)
```

## Guardrails

```python
from agents import Agent, Runner, input_guardrail, GuardrailFunctionOutput

@input_guardrail
async def block_harmful(ctx, agent, input) -> GuardrailFunctionOutput:
    if "harm" in input.lower():
        return GuardrailFunctionOutput(tripwire_triggered=True)
    return GuardrailFunctionOutput(tripwire_triggered=False)

agent = Agent(
    name="Safe Agent",
    instructions="You are a helpful assistant.",
    input_guardrails=[block_harmful],
)
```

## Async execution

```python
import asyncio
from agents import Agent, Runner

async def main():
    agent = Agent(name="Bot", instructions="Help the user.")
    result = await Runner.run(agent, "Hello!")
    print(result.final_output)

asyncio.run(main())
```

## Key parameters

| Parameter | Type | Description |
|---|---|---|
| `name` | str | Agent identifier |
| `instructions` | str or callable | System prompt; callable receives context for dynamic prompts |
| `model` | str | LLM to use (e.g. `"gpt-4o"`, `"gpt-4o-mini"`) |
| `tools` | list | Functions decorated with `@function_tool` or `agent.as_tool()` |
| `handoffs` | list | Agents this agent can delegate to |
| `output_type` | Pydantic model | Forces structured output |
| `input_guardrails` | list | Validation functions run before the agent processes input |
| `output_guardrails` | list | Validation functions run on the final output |

## When to use each pattern

| Pattern | Use when |
|---|---|
| Single agent + tools | Most tasks — one agent calling functions |
| Handoffs | Specialist routing (language, domain, tone) |
| Manager + sub-agents | Parallel work; sub-tasks with independent context |
| Guardrails | Safety checks, schema enforcement, topic filtering |

## Notes

- `Runner.run_sync()` for scripts; `Runner.run()` for async applications.
- `@function_tool` infers the schema from the function signature and docstring — keep docstrings accurate.
- Handoffs transfer conversation control entirely; `as_tool()` keeps control with the caller.
- Full reference: `https://openai.github.io/openai-agents-python/`
