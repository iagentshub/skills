---
id: RESUMIR
name: Resumir
description: "Resumir o extraer texto y transcripciones de URLs, podcasts, YouTube y ficheros locales. Ideal como sustituto de 'transcribe este vídeo'. Requiere el CLI summarize y una clave de API de un proveedor LLM."
icon: 🧾
category: ai
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Resumir

CLI rápido para resumir URLs, ficheros locales y vídeos de YouTube.

Homepage: https://summarize.sh

## Cuándo usar (frases de activación)

Usa esta skill inmediatamente cuando el usuario pregunte algo como:

- "¿De qué trata este enlace/vídeo?"
- "Resume esta URL/artículo"
- "Transcribe este vídeo de YouTube"
- "¿Qué dice este PDF?"

## Inicio rápido

```bash
summarize "https://example.com" --model google/gemini-3-flash-preview
summarize "/ruta/al/fichero.pdf" --model google/gemini-3-flash-preview
summarize "https://youtu.be/dQw4w9WgXcQ" --youtube auto
```

## YouTube: resumen vs transcripción

Transcripción directa (solo URLs):

```bash
summarize "https://youtu.be/dQw4w9WgXcQ" --youtube auto --extract-only
```

Si el usuario pide una transcripción pero es muy larga, devuelve primero un resumen compacto y pregunta qué sección o rango de tiempo expandir.

## Instalación

```bash
brew tap steipete/tap
brew install steipete/tap/summarize
```

## Modelos y claves de API

Establece la clave del proveedor que vayas a usar:

- OpenAI: `OPENAI_API_KEY`
- Anthropic: `ANTHROPIC_API_KEY`
- xAI: `XAI_API_KEY`
- Google: `GEMINI_API_KEY`

El modelo por defecto es `google/gemini-3-flash-preview` si no se especifica ninguno.

## Flags útiles

| Flag | Descripción |
|------|-------------|
| `--length short\|medium\|long\|xl` | Longitud del resumen |
| `--max-output-tokens <n>` | Límite de tokens en la salida |
| `--extract-only` | Solo extraer texto (solo URLs) |
| `--json` | Salida en formato JSON |
| `--firecrawl auto\|off\|always` | Extracción alternativa para sitios bloqueados |
| `--youtube auto` | Fallback via Apify si tienes `APIFY_API_TOKEN` |

## Configuración opcional

Fichero de config: `~/.summarize/config.json`

```json
{ "model": "openai/gpt-5.2" }
```

Servicios opcionales:
- `FIRECRAWL_API_KEY` — para sitios con bloqueos de scraping
- `APIFY_API_TOKEN` — fallback para YouTube cuando falla la extracción directa
