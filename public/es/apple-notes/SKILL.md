---
id: APPLE_NOTES
name: Apple Notes
description: "Gestiona notas de Apple Notes mediante el CLI memo en macOS: crear, ver, editar, eliminar, buscar, mover y exportar notas. Solo disponible en macOS con Notes.app accesible."
icon: 🍎
category: notes
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Apple Notes CLI

Usa `memo notes` para gestionar Apple Notes directamente desde el terminal.
Crea, visualiza, edita, elimina, busca, mueve notas entre carpetas y exporta a HTML/Markdown.

**Solo macOS.** Homepage: https://github.com/antoniorodr/memo

## Instalación

```bash
# Homebrew (recomendado)
brew tap antoniorodr/memo && brew install antoniorodr/memo/memo

# Manual (pip, después de clonar el repo)
pip install .
```

Si se te solicita, concede acceso de Automatización a Notes.app en:
**Preferencias del Sistema → Privacidad y Seguridad → Automatización**

## Ver notas

```bash
# Listar todas las notas
memo notes

# Filtrar por carpeta
memo notes -f "Nombre de Carpeta"

# Buscar notas (búsqueda difusa)
memo notes -s "consulta"
```

## Crear notas

```bash
# Nueva nota con editor interactivo
memo notes -a

# Nueva nota con título rápido
memo notes -a "Título de la nota"
```

## Editar notas

```bash
# Editar nota (selección interactiva)
memo notes -e
```

## Eliminar notas

```bash
# Eliminar nota (selección interactiva)
memo notes -d
```

## Mover notas

```bash
# Mover nota a carpeta (selección interactiva)
memo notes -m
```

## Exportar notas

```bash
# Exportar a HTML/Markdown (selección interactiva)
memo notes -ex
```

## Limitaciones

- No puede editar notas que contengan imágenes o adjuntos.
- Los prompts interactivos requieren acceso a terminal.
- Solo macOS; requiere que Notes.app sea accesible.
