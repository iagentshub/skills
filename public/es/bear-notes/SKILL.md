---
id: BEAR_NOTES
name: Bear Notes (grizzly)
description: Crear, buscar y gestionar notas en Bear en macOS mediante el CLI grizzly. Permite crear notas con etiquetas, leer por ID, añadir texto y listar etiquetas.
icon: 🐻
category: notes
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Bear Notes (grizzly)

> ⚠️ **Solo macOS.** Requiere la app Bear instalada y en ejecución.

Usa `grizzly` para crear, leer y gestionar notas en Bear.

## Requisitos

- macOS con Bear instalado y en ejecución
- `grizzly` CLI instalado

## Instalación

```bash
go install github.com/tylerwince/grizzly/cmd/grizzly@latest
```

## Token de Bear (para algunas operaciones)

Algunas operaciones (añadir texto, listar etiquetas, abrir nota seleccionada) requieren un token de autenticación:

1. Abre Bear → Ayuda → Token API → Copiar Token
2. Guárdalo: `echo "TU_TOKEN" > ~/.config/grizzly/token`

## Comandos comunes

### Crear nota

```bash
# Desde stdin
echo "Contenido de la nota" | grizzly create --title "Mi Nota" --tag trabajo

# Nota vacía
grizzly create --title "Nota rápida" --tag inbox < /dev/null
```

### Abrir/leer nota por ID

```bash
grizzly open-note --id "ID_NOTA" --enable-callback --json
```

### Añadir texto a una nota

```bash
echo "Contenido adicional" | grizzly add-text --id "ID_NOTA" \
  --mode append --token-file ~/.config/grizzly/token
```

### Listar todas las etiquetas

```bash
grizzly tags --enable-callback --json --token-file ~/.config/grizzly/token
```

### Buscar notas por etiqueta

```bash
grizzly open-tag --name "trabajo" --enable-callback --json
```

## Opciones comunes

- `--dry-run` — Vista previa de la URL sin ejecutar
- `--print-url` — Mostrar la URL x-callback-url
- `--enable-callback` — Esperar respuesta de Bear (necesario para leer datos)
- `--json` — Salida JSON (cuando se usan callbacks)
- `--token-file PATH` — Ruta al fichero de token de Bear

## Configuración

Grizzly lee la configuración en este orden:

1. Flags de CLI
2. Variables de entorno (`GRIZZLY_TOKEN_FILE`, `GRIZZLY_CALLBACK_URL`, `GRIZZLY_TIMEOUT`)
3. `.grizzly.toml` en el directorio actual
4. `~/.config/grizzly/config.toml`

Ejemplo de `~/.config/grizzly/config.toml`:

```toml
token_file = "~/.config/grizzly/token"
callback_url = "http://127.0.0.1:42123/success"
timeout = "5s"
```

## Notas

- Bear debe estar abierto para que las operaciones de callback funcionen.
- Los IDs de las notas son UUIDs; obtenlos desde la URL de la nota en Bear.
- Para búsqueda full-text, usa la propia búsqueda de Bear o `open-tag` por etiqueta.
