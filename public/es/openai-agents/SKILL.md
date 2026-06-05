---
id: OPENAI_AGENTS
name: OpenAI Agents SDK
description: "Construir y orquestar agentes de IA con el OpenAI Agents SDK (Python). Usar cuando: se creen agentes con herramientas, handoffs multi-agente, guardrails, salidas estructuradas o sesiones persistentes. NO para: llamadas directas a la API sin agentes, Whisper/TTS, o stacks no Python."
icon: 🤖
category: ai
created_at: "2026-06-05"
updated_at: "2026-06-05"
homepage: https://openai.github.io/openai-agents-python/
---

# OpenAI Agents SDK

Usa el OpenAI Agents SDK para construir, ejecutar y orquestar agentes de IA en Python. Tres primitivas principales: **Agents**, **Handoffs** y **Guardrails**.

## Instalación

```bash
pip install openai-agents
```

Requiere `OPENAI_API_KEY` en el entorno.

## Agente básico

```python
from agents import Agent, Runner

agent = Agent(
    name="Asistente",
    instructions="Eres un asistente útil.",
    model="gpt-4o",
)

result = Runner.run_sync(agent, "¿Cuál es la capital de Francia?")
print(result.final_output)
```

## Agente con herramientas

```python
from agents import Agent, Runner, function_tool

@function_tool
def obtener_clima(ciudad: str) -> str:
    """Devuelve el clima actual para la ciudad dada."""
    return f"El clima en {ciudad} es soleado, 22°C."

agent = Agent(
    name="Agente del Clima",
    instructions="Ayuda con preguntas sobre el clima. Usa obtener_clima cuando se pregunte.",
    tools=[obtener_clima],
)

result = Runner.run_sync(agent, "¿Qué tiempo hace en Madrid?")
print(result.final_output)
```

## Salida estructurada

```python
from pydantic import BaseModel
from agents import Agent, Runner

class Resumen(BaseModel):
    titulo: str
    puntos_clave: list[str]

agent = Agent(
    name="Resumidor",
    instructions="Resume el texto del usuario.",
    output_type=Resumen,
)

result = Runner.run_sync(agent, "Explica la computación cuántica en 3 puntos clave.")
resumen: Resumen = result.final_output
print(resumen.titulo, resumen.puntos_clave)
```

## Multi-agente: handoffs

```python
from agents import Agent, Runner, handoff

agente_ingles = Agent(
    name="Agente Inglés",
    instructions="Solo respondes en inglés.",
)

triaje = Agent(
    name="Triaje",
    instructions="Deriva al agente inglés para hablantes de inglés.",
    handoffs=[handoff(agente_ingles)],
)

result = Runner.run_sync(triaje, "Hello, I need help.")
print(result.final_output)
```

## Multi-agente: patrón manager

```python
from agents import Agent, Runner

investigador = Agent(name="Investigador", instructions="Investiga hechos.")
escritor = Agent(name="Escritor", instructions="Escribe basándote en la investigación.")

# El manager usa sub-agentes como tools via as_tool()
manager = Agent(
    name="Manager",
    instructions="Coordina investigación y escritura.",
    tools=[investigador.as_tool(), escritor.as_tool()],
)
```

## Guardrails

```python
from agents import Agent, Runner, input_guardrail, GuardrailFunctionOutput

@input_guardrail
async def bloquear_dañino(ctx, agent, input) -> GuardrailFunctionOutput:
    if "daño" in input.lower():
        return GuardrailFunctionOutput(tripwire_triggered=True)
    return GuardrailFunctionOutput(tripwire_triggered=False)

agent = Agent(
    name="Agente Seguro",
    instructions="Eres un asistente útil.",
    input_guardrails=[bloquear_dañino],
)
```

## Ejecución asíncrona

```python
import asyncio
from agents import Agent, Runner

async def main():
    agent = Agent(name="Bot", instructions="Ayuda al usuario.")
    result = await Runner.run(agent, "¡Hola!")
    print(result.final_output)

asyncio.run(main())
```

## Parámetros principales

| Parámetro | Tipo | Descripción |
|---|---|---|
| `name` | str | Identificador del agente |
| `instructions` | str o callable | System prompt; callable recibe contexto para prompts dinámicos |
| `model` | str | LLM a usar (ej: `"gpt-4o"`, `"gpt-4o-mini"`) |
| `tools` | list | Funciones con `@function_tool` o `agent.as_tool()` |
| `handoffs` | list | Agentes a los que este agente puede delegar |
| `output_type` | modelo Pydantic | Fuerza salida estructurada |
| `input_guardrails` | list | Funciones de validación que se ejecutan antes de procesar el input |
| `output_guardrails` | list | Funciones de validación que se ejecutan sobre la salida final |

## Cuándo usar cada patrón

| Patrón | Usar cuando |
|---|---|
| Agente único + herramientas | La mayoría de tareas — un agente llamando funciones |
| Handoffs | Enrutamiento a especialistas (idioma, dominio, tono) |
| Manager + sub-agentes | Trabajo paralelo; sub-tareas con contexto independiente |
| Guardrails | Controles de seguridad, esquemas, filtros temáticos |

## Notas

- `Runner.run_sync()` para scripts; `Runner.run()` para aplicaciones asíncronas.
- `@function_tool` infiere el esquema de la firma y docstring — mantén los docstrings precisos.
- Los handoffs transfieren el control de la conversación completamente; `as_tool()` mantiene el control en el llamador.
- Referencia completa: `https://openai.github.io/openai-agents-python/`
