---
id: THINGS_MAC
name: Gestión de Tareas en Things 3
description: Gestionar Things 3 en macOS mediante el CLI things. Leer inbox/today/búsqueda desde la base de datos local y añadir/actualizar todos mediante el esquema de URLs de Things.
icon: ✅
category: productivity
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Gestión de Tareas en Things 3

> ⚠️ **Solo macOS.** Requiere Things 3 instalado.

Usa `things` para leer la base de datos local de Things (inbox, today, búsqueda, proyectos, áreas, etiquetas) y para añadir/actualizar todos mediante el esquema de URLs de Things.

## Requisitos

- macOS con Things 3 instalado
- `things` CLI instalado

## Instalación

```bash
GOBIN=/opt/homebrew/bin go install github.com/ossianhempel/things3-cli/cmd/things@latest
```

Si las lecturas de la base de datos fallan, concede **Acceso Completo al Disco** a la app que ejecuta el CLI (Terminal para uso manual; la app de tu agente para uso automático).

## Configuración opcional

```bash
# Variable de entorno para la carpeta de la base de datos
export THINGSDB=/ruta/a/ThingsData-xxx

# Token para operaciones de escritura
export THINGS_AUTH_TOKEN=tu-token-aqui
```

## Lectura (base de datos local)

```bash
# Bandeja de entrada
things inbox --limit 50

# Tareas de hoy
things today

# Próximas tareas
things upcoming

# Buscar tareas
things search "dentista"

# Proyectos, áreas y etiquetas
things projects
things areas
things tags
```

## Escritura (esquema de URL)

```bash
# Previsualización sin ejecutar
things --dry-run add "Comprar pan"

# Añadir tarea básica
things add "Comprar pan"

# Con notas
things add "Comprar leche" --notes "Desnatada + zumo"

# En un proyecto o área
things add "Reservar vuelo" --list "Viajes"

# Con sección dentro del proyecto
things add "Cargar portátil" --list "Viajes" --heading "Antes de salir"

# Con etiquetas
things add "Llamar al médico" --tags "salud,teléfono"

# Con checklist
things add "Preparar viaje" --checklist-item "Pasaporte" --checklist-item "Billetes"

# Con fecha y vencimiento
things add "Entregar informe" --when today --deadline 2026-05-01
```

## Modificar tarea (requiere token de autenticación)

```bash
# Obtener el UUID de la tarea
things search "leche" --limit 5

# Cambiar título
things update --id <UUID> --auth-token <TOKEN> "Nuevo título"

# Añadir notas
things update --id <UUID> --auth-token <TOKEN> --append-notes "Nota extra"

# Mover a proyecto
things update --id <UUID> --auth-token <TOKEN> --list "Otro proyecto"

# Marcar como completada
things update --id <UUID> --auth-token <TOKEN> --completed

# Cancelar
things update --id <UUID> --auth-token <TOKEN> --canceled
```

## Notas

- Las eliminaciones no están soportadas por este CLI; usa la UI de Things o marca como completada/cancelada.
- `--dry-run` imprime la URL sin abrirla; útil para depurar.
- El token de Things se obtiene en: Things → Preferencias → API → Copiar Token.
