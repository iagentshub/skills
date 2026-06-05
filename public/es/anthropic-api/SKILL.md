---
id: ANTHROPIC_API
name: Anthropic API
description: "Construir agentes y herramientas con la API de Anthropic Claude (Python/TypeScript). Usar cuando: se definan herramientas cliente, se implementen bucles agénticos, se use computer use, se construya con tool_use, o se integre Claude en aplicaciones. NO para: Gemini, OpenAI, ni creación de servidores MCP."
icon: 🧠
category: ai
created_at: "2026-06-05"
updated_at: "2026-06-05"
homepage: https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/overview
---

# Anthropic API — Agentes y Tool Use

Usa el SDK de Anthropic para construir agentes con llamadas a herramientas, bucles agénticos y salidas estructuradas.

## Instalación

```bash
pip install anthropic          # Python
npm install @anthropic-ai/sdk  # TypeScript/Node
```

Requiere `ANTHROPIC_API_KEY` en el entorno.

## Cómo funciona el tool use

1. Defines herramientas (nombre, descripción, esquema de entrada).
2. Claude decide si llamar una herramienta según el mensaje del usuario.
3. Si Claude llama una herramienta (`stop_reason: "tool_use"`), tu código la ejecuta.
4. Envías el resultado como `tool_result`.
5. Claude usa el resultado para continuar.

**Herramientas cliente** — tu código las ejecuta (consultas a BD, archivos, APIs externas).
**Herramientas servidor** — Anthropic las ejecuta directamente (web_search, code_execution, web_fetch).

## Ejemplo básico (Python)

```python
import anthropic

client = anthropic.Anthropic()

tools = [
    {
        "name": "obtener_clima",
        "description": "Obtiene el clima actual para una ubicación.",
        "input_schema": {
            "type": "object",
            "properties": {
                "ubicacion": {"type": "string", "description": "Nombre de la ciudad"}
            },
            "required": ["ubicacion"]
        }
    }
]

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": "¿Qué tiempo hace en París?"}],
)
print(response.content)
```

## Bucle agéntico (Python)

```python
import anthropic

client = anthropic.Anthropic()

def obtener_clima(ubicacion: str) -> str:
    return f"El tiempo en {ubicacion} es 18°C y nublado."

def ejecutar_agente(mensaje_usuario: str):
    messages = [{"role": "user", "content": mensaje_usuario}]
    tools = [
        {
            "name": "obtener_clima",
            "description": "Obtiene el clima actual para una ubicación.",
            "input_schema": {
                "type": "object",
                "properties": {
                    "ubicacion": {"type": "string"}
                },
                "required": ["ubicacion"]
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

        # Procesar llamadas a herramientas
        resultados = []
        for block in response.content:
            if block.type == "tool_use":
                if block.name == "obtener_clima":
                    resultado = obtener_clima(**block.input)
                    resultados.append({
                        "type": "tool_result",
                        "tool_use_id": block.id,
                        "content": resultado,
                    })

        messages.append({"role": "assistant", "content": response.content})
        messages.append({"role": "user", "content": resultados})

resultado = ejecutar_agente("¿Qué tiempo hace en Madrid y Londres?")
print(resultado)
```

## Herramientas servidor (sin código de ejecución)

```python
# web_search — Anthropic la ejecuta
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    tools=[{"type": "web_search_20260209", "name": "web_search"}],
    messages=[{"role": "user", "content": "Últimas noticias sobre agentes de IA"}],
)

# code_execution — ejecuta Python en un sandbox
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=4096,
    tools=[{"type": "code_execution_20250522", "name": "code_execution"}],
    messages=[{"role": "user", "content": "Calcula los primeros 10 números de Fibonacci"}],
)
```

## Salida estructurada (JSON)

```python
tools = [
    {
        "name": "extraer_persona",
        "description": "Extrae datos de una persona del texto.",
        "input_schema": {
            "type": "object",
            "properties": {
                "nombre": {"type": "string"},
                "edad": {"type": "integer"},
                "email": {"type": "string"}
            },
            "required": ["nombre"]
        }
    }
]

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=512,
    tools=tools,
    tool_choice={"type": "tool", "name": "extraer_persona"},  # Fuerza esta herramienta
    messages=[{"role": "user", "content": "Juan García, 32, juan@ejemplo.com"}],
)

# El input de la herramienta ES los datos estructurados
tool_call = next(b for b in response.content if b.type == "tool_use")
persona = tool_call.input  # {"nombre": "Juan García", "edad": 32, "email": "..."}
```

## Tool use estricto

```python
tools = [
    {
        "name": "mi_herramienta",
        "description": "...",
        "input_schema": {...},
        "strict": True  # Las llamadas de Claude siempre coinciden exactamente con el esquema
    }
]
```

## Controlar cuándo se usan las herramientas

```python
# Forzar una herramienta específica
tool_choice={"type": "tool", "name": "obtener_clima"}

# Requerir al menos una herramienta
tool_choice={"type": "any"}

# Dejar que Claude decida (por defecto)
tool_choice={"type": "auto"}

# Nunca llamar herramientas
tool_choice={"type": "none"}
```

## Ejemplo TypeScript

```typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();

const response = await client.messages.create({
  model: "claude-opus-4-8",
  max_tokens: 1024,
  tools: [
    {
      name: "obtener_clima",
      description: "Obtiene el clima actual para una ubicación.",
      input_schema: {
        type: "object",
        properties: {
          ubicacion: { type: "string" }
        },
        required: ["ubicacion"]
      }
    }
  ],
  messages: [{ role: "user", content: "¿Tiempo en Tokio?" }]
});
```

## Modelos actuales

| Modelo | ID | Ideal para |
|---|---|---|
| Claude Opus 4.8 | `claude-opus-4-8` | Razonamiento complejo, agentes |
| Claude Sonnet 4.6 | `claude-sonnet-4-6` | Balance velocidad/capacidad |
| Claude Haiku 4.5 | `claude-haiku-4-5-20251001` | Tareas rápidas y de alto volumen |

## Notas

- Siempre verifica `stop_reason` antes de leer `content` — es `"tool_use"` cuando se llaman herramientas, `"end_turn"` cuando termina.
- Las descripciones de las herramientas son el campo más importante — la decisión de Claude de llamar una herramienta depende casi completamente de la descripción.
- En bucles agénticos, añade tanto la respuesta del asistente como los resultados de las herramientas a `messages` antes de la siguiente llamada a la API.
- Referencia completa: `https://docs.anthropic.com/`
