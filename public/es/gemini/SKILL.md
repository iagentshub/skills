---
id: GEMINI
name: Gemini CLI
description: CLI de Gemini para consultas, resúmenes y generación de contenido en modo one-shot. Usar cuando se necesite consultar el modelo Gemini de Google directamente desde el terminal.
icon: ✨
category: ai
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Gemini CLI

Usa el CLI de Gemini en modo one-shot para responder preguntas, generar contenido y resumir información. Evita el modo interactivo.

## Requisitos

- `gemini` CLI instalado
- Autenticación configurada (ejecuta `gemini` una vez interactivamente y sigue el flujo de login)

## Instalación

```bash
brew install gemini-cli
```

## Uso básico

```bash
# Consulta simple
gemini "¿Cuál es la capital de Francia?"

# Con modelo específico
gemini --model gemini-2.5-pro "Explica la teoría de la relatividad en 3 párrafos"

# Respuesta en JSON
gemini --output-format json "Devuelve un JSON con las 5 capitales de Europa más pobladas"
```

## Extensiones

```bash
# Listar extensiones disponibles
gemini --list-extensions

# Gestionar extensiones
gemini extensions --help
```

## Notas

- La primera vez, ejecuta `gemini` de forma interactiva para completar el login con Google.
- Usa siempre modo one-shot (prompt como argumento); evita el modo interactivo en scripts y agentes.
- Evita `--yolo` por razones de seguridad.
- Referencia completa: `gemini --help`.
