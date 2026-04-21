---
id: MONITOR_BLOGS
name: Monitor de Blogs y Feeds RSS
description: Monitorizar blogs y feeds RSS/Atom para detectar nuevas publicaciones usando el CLI blogwatcher. Permite añadir blogs, escanear actualizaciones y marcar artículos como leídos.
icon: 📰
category: data
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Monitor de Blogs y Feeds RSS (blogwatcher)

Usa `blogwatcher` para hacer seguimiento de blogs y feeds RSS/Atom y detectar nuevas publicaciones.

## Requisitos

- `blogwatcher` instalado

## Instalación

```bash
go install github.com/Hyaxia/blogwatcher/cmd/blogwatcher@latest
```

## Comandos principales

```bash
# Añadir un blog/feed
blogwatcher add "Nombre Blog" https://ejemplo.com
blogwatcher add "Hacker News" https://news.ycombinator.com/rss

# Listar blogs monitorizados
blogwatcher blogs

# Escanear actualizaciones
blogwatcher scan

# Ver artículos (incluye los nuevos)
blogwatcher articles

# Marcar artículo como leído (por ID)
blogwatcher read 1

# Marcar todos como leídos
blogwatcher read-all

# Eliminar un blog
blogwatcher remove "Nombre Blog"
```

## Ejemplo de flujo de uso

```bash
# 1. Añadir fuentes de interés
blogwatcher add "Blog de Python" https://pythonweekly.com/rss

# 2. Escanear al inicio del día
blogwatcher scan

# 3. Ver novedades
blogwatcher articles

# 4. Marcar como leídos
blogwatcher read-all
```

## Ejemplo de salida

```
$ blogwatcher blogs
Blogs monitorizados (2):

  Hacker News
    URL: https://news.ycombinator.com/rss

  Blog Python
    URL: https://pythonweekly.com/rss

$ blogwatcher scan
Escaneando 2 blog(s)...

  Hacker News
    Fuente: RSS | Encontrados: 30 | Nuevos: 5

¡Se encontraron 5 artículo(s) nuevos en total!
```

## Notas

- Usa `blogwatcher <comando> --help` para ver flags y opciones adicionales.
- Los feeds RSS/Atom son los más compatibles; algunos blogs requieren la URL del feed específico (p.ej. `/feed`, `/rss`, `/atom.xml`).
