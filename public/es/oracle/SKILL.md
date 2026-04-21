---
id: ORACLE
name: Oracle CLI (consulta con contexto de ficheros)
description: Bundlear un prompt con ficheros seleccionados para hacer una consulta one-shot a un modelo de lenguaje grande con contexto real del repositorio. Útil para análisis profundos que requieren leer mucho código.
icon: 🧿
category: dev
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Oracle CLI

Oracle empaqueta tu prompt y los ficheros seleccionados en una consulta "one-shot" para que otro modelo responda con contexto real del repositorio. Trata la respuesta como orientativa: verifica siempre contra el código y los tests.

## Requisitos

- `oracle` instalado

## Instalación

```bash
npm install -g @steipete/oracle
# o sin instalar
npx -y @steipete/oracle --help
```

## Flujo recomendado

1. Elige un conjunto mínimo de ficheros (los que contienen la verdad relevante).
2. Previsualiza el payload y el coste en tokens (`--dry-run` + `--files-report`).
3. Ejecuta la consulta con el motor y modelo adecuados.
4. Si la sesión se desconecta/caduca, reconéctate a la sesión guardada (no vuelvas a lanzar).

## Comandos

### Ayuda

```bash
oracle --help
npx -y @steipete/oracle --help   # sin instalación previa
```

### Previsualización (sin consumir tokens)

```bash
# Resumen sin ejecutar
oracle --dry-run summary -p "Describe qué hace esta función" --file "src/**"

# Con informe de tokens
oracle --dry-run summary --files-report -p "Tarea" --file "src/**"
```

### Ejecutar consulta

```bash
# Modo API (rápido, coste por tokens)
oracle -p "¿Qué hace esta función y cómo se puede mejorar?" \
  --file "app/backend/engine/runner.py" \
  --file "app/backend/executors/**"

# Solo renderizar y copiar al portapapeles (para pegar manualmente)
oracle --render --copy -p "Tarea" --file "src/**"
```

## Adjuntar ficheros (`--file`)

`--file` acepta ficheros, directorios y globs. Se puede usar varias veces:

```bash
# Incluir directorio completo
oracle -p "..." --file "app/backend/"

# Múltiples rutas
oracle -p "..." --file "app/backend/engine/" --file "README.md"

# Excluir ficheros
oracle -p "..." --file "app/**" --file "!app/**/*.pyc" --file "!**/__pycache__/**"
```

**Directorios ignorados por defecto:** `node_modules`, `dist`, `coverage`, `.git`, `build`, `tmp`.
Respeta `.gitignore` al expandir globs. No sigue enlaces simbólicos.

## Notas

- Elige el conjunto mínimo de ficheros que contengan la información necesaria; más ficheros = más tokens.
- El modo `--render --copy` genera el prompt en el portapapeles para pegarlo manualmente en cualquier chat.
- Usa `--dry-run` siempre antes de lanzar consultas grandes para estimar el coste.
- Trata las respuestas como orientativas; verifica siempre en el código real.
