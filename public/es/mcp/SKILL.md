---
id: MCP
name: MCP Server
description: "Crear servidores Model Context Protocol (MCP) para exponer herramientas, recursos y prompts a clientes de IA. Usar cuando: se construyan servidores MCP personalizados en Python o TypeScript, se conecten clientes de IA a sistemas externos, o se creen integraciones de herramientas reutilizables. NO para: configuración de clientes MCP, ni integraciones API no-MCP."
icon: 🔌
category: dev
created_at: "2026-06-05"
updated_at: "2026-06-05"
homepage: https://modelcontextprotocol.io/
---

# MCP — Model Context Protocol

Construye servidores MCP para conectar clientes de IA (Claude, ChatGPT, VS Code, Cursor) a tus datos y herramientas mediante un protocolo estándar.

## Qué ofrece MCP

Un servidor MCP puede exponer tres tipos de capacidades:

| Primitiva | Descripción | Ejemplo |
|---|---|---|
| **Tools** | Funciones que el LLM puede llamar | `buscar_bd`, `enviar_email` |
| **Resources** | Datos similares a archivos que el cliente puede leer | Contenido de archivos, respuestas de API |
| **Prompts** | Plantillas predefinidas para usuarios | `resumir-pr`, `revisar-codigo` |

## Instalación

```bash
# Python
pip install mcp

# TypeScript/Node
npm install @modelcontextprotocol/sdk
```

## Servidor mínimo (Python)

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("mi-servidor")

@mcp.tool()
def sumar(a: int, b: int) -> int:
    """Suma dos números."""
    return a + b

@mcp.tool()
def obtener_clima(ciudad: str) -> str:
    """Obtiene el clima actual para una ciudad."""
    # La implementación real llamaría a una API del clima
    return f"El clima en {ciudad} es soleado, 22°C."

if __name__ == "__main__":
    mcp.run()
```

## Servidor mínimo (TypeScript)

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

const server = new McpServer({ name: "mi-servidor", version: "1.0.0" });

server.tool(
  "sumar",
  { a: z.number(), b: z.number() },
  async ({ a, b }) => ({
    content: [{ type: "text", text: String(a + b) }]
  })
);

const transport = new StdioServerTransport();
await server.connect(transport);
```

## Agregar recursos (Python)

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("mi-servidor")

@mcp.resource("archivo://{ruta}")
def leer_archivo(ruta: str) -> str:
    """Lee un archivo y devuelve su contenido."""
    with open(ruta) as f:
        return f.read()

@mcp.resource("bd://usuarios/{user_id}")
def obtener_usuario(user_id: str) -> str:
    """Obtiene datos de usuario de la base de datos."""
    # Consulta tu base de datos aquí
    return f'{{"id": "{user_id}", "nombre": "Ana"}}'
```

## Agregar prompts (Python)

```python
from mcp.server.fastmcp import FastMCP
from mcp.types import PromptMessage, TextContent

mcp = FastMCP("mi-servidor")

@mcp.prompt()
def resumir_pr(numero_pr: int, repo: str) -> list[PromptMessage]:
    """Prompt para resumir un pull request."""
    return [
        PromptMessage(
            role="user",
            content=TextContent(
                type="text",
                text=f"Resume el pull request #{numero_pr} en {repo}. Enfócate en: qué cambió, por qué y riesgos potenciales."
            )
        )
    ]
```

## Conectar a Claude Desktop

Añadir a `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS):

```json
{
  "mcpServers": {
    "mi-servidor": {
      "command": "python",
      "args": ["/ruta/a/mi_servidor.py"]
    }
  }
}
```

Para un servidor Node.js:

```json
{
  "mcpServers": {
    "mi-servidor": {
      "command": "node",
      "args": ["/ruta/a/mi_servidor.js"]
    }
  }
}
```

## Conectar a Claude Code (CLI)

```bash
# Añadir un servidor local
claude mcp add mi-servidor -- python /ruta/a/mi_servidor.py

# Añadir con variables de entorno
claude mcp add mi-servidor -e API_KEY=abc123 -- python /ruta/a/mi_servidor.py

# Listar servidores conectados
claude mcp list

# Eliminar un servidor
claude mcp remove mi-servidor
```

## Ejemplo real: servidor de consulta a base de datos

```python
import sqlite3
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("servidor-sqlite")
RUTA_BD = "miapp.db"

@mcp.tool()
def consultar_base_datos(sql: str) -> list[dict]:
    """Ejecuta una consulta SQL de solo lectura y devuelve los resultados como lista de registros."""
    conn = sqlite3.connect(RUTA_BD)
    conn.row_factory = sqlite3.Row
    cursor = conn.execute(sql)
    filas = [dict(fila) for fila in cursor.fetchall()]
    conn.close()
    return filas

@mcp.tool()
def listar_tablas() -> list[str]:
    """Lista todas las tablas de la base de datos."""
    conn = sqlite3.connect(RUTA_BD)
    cursor = conn.execute("SELECT name FROM sqlite_master WHERE type='table'")
    tablas = [fila[0] for fila in cursor.fetchall()]
    conn.close()
    return tablas

if __name__ == "__main__":
    mcp.run()
```

## Guía de diseño de herramientas

- **Nombre**: minúsculas, separado por guiones bajos, comenzar con verbo (`obtener_usuario`, `buscar_docs`, `enviar_mensaje`)
- **Descripción**: una frase explicando qué hace y cuándo llamarla — esto es lo que lee el LLM
- **Parámetros**: usa type hints; añade descripciones para parámetros no obvios
- Mantén cada herramienta enfocada en una operación — compón en el cliente, no en el servidor

## Tipos de transporte

| Transporte | Caso de uso |
|---|---|
| `stdio` | Servidores locales lanzados como subprocesos (por defecto para la mayoría de clientes) |
| `SSE` | Servidores remotos sobre HTTP (para despliegues en la nube) |

## Notas

- Los servidores MCP son sin estado por sesión por defecto; usa una base de datos o archivo si necesitas persistencia.
- La clase `FastMCP` (Python) infiere los esquemas de herramientas de los type hints y docstrings automáticamente.
- Cualquier cliente MCP puede usar tu servidor sin modificaciones — construye una vez, úsalo en cualquier lugar.
- Referencia completa: `https://modelcontextprotocol.io/docs/develop/build-server`
