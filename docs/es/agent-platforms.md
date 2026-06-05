<div align="center">
  <a href="index.md">← Índice</a> &nbsp;·&nbsp;
  <a href="../en/agent-platforms.md">🇬🇧 View in English</a>
</div>

<br>

# Referencia de Plataformas de Agentes

Comparativa de las cuatro principales plataformas para construir y desplegar agentes de IA, y cómo mover configuraciones entre ellas.

---

## Resumen

| Plataforma | Uso principal | Lenguaje | Primitiva clave |
|---|---|---|---|
| [OpenAI Agents SDK](#openai-agents-sdk) | Agentes Python orquestados | Python | `Agent` + `Runner` |
| [Anthropic API](#anthropic-api) | Agentes Claude con tool use | Python, TypeScript | `messages.create` + tools |
| [MCP](#mcp-model-context-protocol) | Servidores de herramientas reutilizables para cualquier cliente | Python, TypeScript | `@mcp.tool()` |
| [GitHub Copilot Skills](#github-copilot-skills) | Automatización IDE/flujo para Copilot | Markdown + shell | `SKILL.md` |

---

## OpenAI Agents SDK

**Cuándo elegirlo:** Quieres un framework Python de alto nivel con orquestación multi-agente integrada, handoffs, guardrails y sesiones.

**Conceptos clave:**
- `Agent(name, instructions, tools, handoffs, output_type)` — define un agente
- `@function_tool` — envuelve una función Python como herramienta del agente
- `Runner.run_sync()` / `Runner.run()` — ejecuta el bucle del agente
- `handoff(agent)` — delega la conversación a un agente especialista
- `agent.as_tool()` — expone un agente como herramienta llamable (patrón manager)

**Skill:** [openai-agents](../public/es/openai-agents/SKILL.md)
**Docs:** https://openai.github.io/openai-agents-python/

---

## Anthropic API

**Cuándo elegirlo:** Necesitas control directo sobre la API de Claude, bucles agénticos personalizados, herramientas del lado servidor (web_search, code_execution), o soporte TypeScript.

**Conceptos clave:**
- Herramientas definidas como esquemas JSON con `name`, `description`, `input_schema`
- `stop_reason: "tool_use"` indica que Claude quiere llamar una herramienta
- Los bloques `tool_result` envían resultados de ejecución de vuelta a Claude
- `tool_choice` controla cuándo se usan las herramientas (`auto`, `any`, `tool`, `none`)
- `strict: true` garantiza conformidad exacta con el esquema

**Tipos de herramientas:**

| Tipo | Ejemplos | Ejecutado por |
|---|---|---|
| Herramientas cliente | Funciones personalizadas, consultas a BD | Tu código |
| Herramientas servidor | `web_search`, `code_execution`, `web_fetch` | Anthropic |

**Skill:** [anthropic-api](../public/es/anthropic-api/SKILL.md)
**Docs:** https://docs.anthropic.com/

---

## MCP (Model Context Protocol)

**Cuándo elegirlo:** Quieres construir una integración reutilizable que funcione con cualquier cliente MCP compatible (Claude, ChatGPT, VS Code, Cursor) sin reescribirla por plataforma.

**Conceptos clave:**
- **Tools** — funciones llamables (`@mcp.tool()`)
- **Resources** — datos legibles (`@mcp.resource("esquema://{param}")`)
- **Prompts** — plantillas de prompts reutilizables (`@mcp.prompt()`)
- **Transport** — `stdio` (local) o `SSE` (remoto/nube)

**Configuración de clientes:**

```json
// Claude Desktop / Claude Code
{ "mcpServers": { "nombre": { "command": "python", "args": ["servidor.py"] } } }
```

**Skill:** [mcp](../public/es/mcp/SKILL.md)
**Docs:** https://modelcontextprotocol.io/

---

## GitHub Copilot Skills

**Cuándo elegirlo:** Quieres extender GitHub Copilot Cloud Agent con instrucciones personalizadas, scripts o flujos de trabajo — vinculados a un repositorio o perfil de usuario.

**Conceptos clave:**
- Skills = `SKILL.md` + scripts opcionales en una carpeta dedicada
- Copilot carga las skills automáticamente cuando el prompt coincide con la descripción
- `allowed-tools: [shell]` preaprueba la ejecución de shell
- Gestionadas con el CLI `gh skill` (buscar, instalar, actualizar, publicar)

**Alcance:**

| Ubicación | Alcance |
|---|---|
| `.github/skills/` | Compartido con el equipo, versionado |
| `~/.copilot/skills/` | Solo personal |

**Skill:** [github-copilot-skills](../public/es/github-copilot-skills/SKILL.md)
**Docs:** https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-skills

---

## Elegir la plataforma correcta

```
¿Necesitas un servidor de herramientas reutilizable para cualquier cliente IA?
  → MCP

¿Construyes directamente con Claude (Anthropic)?
  → anthropic-api

¿Construyes un sistema multi-agente Python (OpenAI)?
  → openai-agents

¿Extiendes GitHub Copilot con flujos personalizados?
  → github-copilot-skills
```

---

## Mover configuraciones entre plataformas

La arquitectura del hub permite compartir lógica de agentes entre plataformas. Patrones comunes:

### Herramienta → servidor MCP
Una herramienta definida en Python para una plataforma puede convertirse en servidor MCP, haciéndola disponible para todos los clientes MCP:

```python
# Original: herramienta cliente Anthropic
def buscar_docs(consulta: str) -> list[dict]: ...

# Como servidor MCP — sin cambios en la lógica
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("servidor-docs")

@mcp.tool()
def buscar_docs(consulta: str) -> list[dict]:
    """Busca en la documentación interna."""
    ...  # misma implementación
```

### Instrucciones de agente → skill de Copilot
La descripción e instrucciones de cualquier agente pueden empaquetarse como skill de Copilot:

```markdown
---
name: nombre-del-agente
description: "Qué hace el agente y cuándo debe activarlo Copilot."
---

# Nombre del Agente

[Instrucciones del agente aquí]
```

### Herramienta OpenAI → herramienta Anthropic
Ambas plataformas usan esquemas JSON similares; solo difiere el envoltorio:

```python
# OpenAI Agents SDK
@function_tool
def obtener_usuario(user_id: str) -> dict: ...

# Equivalente en Anthropic API
{
  "name": "obtener_usuario",
  "description": "Obtiene un registro de usuario por ID.",
  "input_schema": {
    "type": "object",
    "properties": {"user_id": {"type": "string"}},
    "required": ["user_id"]
  }
}
```
