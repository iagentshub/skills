---
id: OBSIDIAN
name: Obsidian
description: "Trabaja con vaults de Obsidian (notas en Markdown plano) y automatiza operaciones con obsidian-cli: buscar, crear, mover, renombrar y eliminar notas. Solo para macOS/Linux con Obsidian instalado."
icon: 💎
category: notes
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Obsidian

Un vault de Obsidian es simplemente una carpeta en disco con ficheros Markdown.

Homepage: https://help.obsidian.md

## Estructura del vault (típica)

- Notas: `*.md` (Markdown plano; editable con cualquier editor)
- Config: `.obsidian/` (ajustes del workspace y plugins; normalmente no tocar)
- Canvases: `*.canvas` (JSON)
- Adjuntos: la carpeta que hayas configurado en Obsidian (imágenes, PDFs, etc.)

## Instalación

```bash
brew install yakitrak/yakitrak/obsidian-cli
```

## Encontrar el vault activo

Obsidian guarda los vaults en:
- macOS: `~/Library/Application Support/obsidian/obsidian.json`

`obsidian-cli` resuelve vaults desde ese fichero; el nombre del vault es normalmente el nombre de la carpeta.

```bash
# Si ya tienes configurado un vault por defecto:
obsidian-cli print-default --path-only

# Si no, lee el fichero de configuración:
cat ~/Library/Application\ Support/obsidian/obsidian.json
```

> No asumas rutas de vault hardcodeadas; lee siempre la configuración o usa `print-default`.

## obsidian-cli: inicio rápido

### Configurar vault por defecto (una vez)

```bash
obsidian-cli set-default "nombre-de-mi-vault"
obsidian-cli print-default
```

### Buscar notas

```bash
# Buscar por nombre
obsidian-cli search "consulta"

# Buscar por contenido (muestra fragmentos y líneas)
obsidian-cli search-content "consulta"
```

### Crear notas

```bash
obsidian-cli create "Carpeta/Nueva nota" --content "Contenido inicial" --open
```

> Requiere que el gestor URI de Obsidian (`obsidian://...`) funcione (Obsidian instalado).
> Evita crear notas bajo carpetas ocultas (ej. `.algo/...`); Obsidian puede rechazarlo.

### Mover/renombrar (refactorización segura)

```bash
obsidian-cli move "ruta/antigua/nota" "ruta/nueva/nota"
```

Actualiza `[[wikilinks]]` y enlaces Markdown a lo largo del vault — esta es la ventaja principal frente a `mv`.

### Eliminar notas

```bash
obsidian-cli delete "ruta/nota"
```

## Notas

- Los vaults múltiples son habituales (iCloud vs `~/Documents`, trabajo/personal, etc.). No adivines; lee la config.
- Para ediciones directas, abre el fichero `.md` y cámbialo; Obsidian lo detectará automáticamente.
- Evita escribir rutas de vault hardcodeadas en scripts; prefiere leer la config o usar `print-default`.
