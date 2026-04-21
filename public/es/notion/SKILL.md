---
id: NOTION
name: Notion
description: "API de Notion para crear y gestionar páginas, bases de datos y bloques. Requiere NOTION_API_KEY. Úsalo para crear/leer/actualizar páginas y fuentes de datos (databases) en Notion."
icon: 📝
category: notes
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Notion

Usa la API de Notion para crear/leer/actualizar páginas, fuentes de datos (databases) y bloques.

Homepage: https://developers.notion.com

## Configuración

1. Crea una integración en https://notion.so/my-integrations
2. Copia la clave de API (empieza por `ntn_` o `secret_`)
3. Guárdala como variable de entorno:

```bash
export NOTION_API_KEY="ntn_tu_clave_aqui"
```

4. Comparte las páginas/bases de datos objetivo con tu integración (clic en "..." → "Conectar a" → tu integración)

## Cabeceras de autenticación

Todas las peticiones necesitan:

```bash
curl -X GET "https://api.notion.com/v1/..." \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json"
```

> **Nota:** La cabecera `Notion-Version` es obligatoria. Esta skill usa `2025-09-03` (la más reciente).

## Operaciones comunes

### Buscar páginas y bases de datos

```bash
curl -X POST "https://api.notion.com/v1/search" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{"query": "título de la página"}'
```

### Obtener una página

```bash
curl "https://api.notion.com/v1/pages/{page_id}" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03"
```

### Obtener el contenido de una página (bloques)

```bash
curl "https://api.notion.com/v1/blocks/{page_id}/children" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03"
```

### Crear una página en una base de datos

```bash
curl -X POST "https://api.notion.com/v1/pages" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{
    "parent": {"database_id": "xxx"},
    "properties": {
      "Name": {"title": [{"text": {"content": "Nuevo elemento"}}]},
      "Status": {"select": {"name": "Por hacer"}}
    }
  }'
```

### Consultar una base de datos

```bash
curl -X POST "https://api.notion.com/v1/data_sources/{data_source_id}/query" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{
    "filter": {"property": "Status", "select": {"equals": "Activo"}},
    "sorts": [{"property": "Fecha", "direction": "descending"}]
  }'
```

### Actualizar propiedades de una página

```bash
curl -X PATCH "https://api.notion.com/v1/pages/{page_id}" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{"properties": {"Status": {"select": {"name": "Hecho"}}}}'
```

### Añadir bloques a una página

```bash
curl -X PATCH "https://api.notion.com/v1/blocks/{page_id}/children" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{
    "children": [
      {"object": "block", "type": "paragraph", "paragraph": {"rich_text": [{"text": {"content": "Hola desde Gaia"}}]}}
    ]
  }'
```

## Tipos de propiedades

Formatos comunes para propiedades de bases de datos:

- **Título:** `{"title": [{"text": {"content": "..."}}]}`
- **Texto enriquecido:** `{"rich_text": [{"text": {"content": "..."}}]}`
- **Selección:** `{"select": {"name": "Opción"}}`
- **Multi-selección:** `{"multi_select": [{"name": "A"}, {"name": "B"}]}`
- **Fecha:** `{"date": {"start": "2026-04-18"}}`
- **Casilla:** `{"checkbox": true}`
- **Número:** `{"number": 42}`
- **URL:** `{"url": "https://..."}`
- **Email:** `{"email": "a@ejemplo.com"}`

## Notas

- Los IDs de página/base de datos son UUIDs (con o sin guiones).
- La API no puede establecer filtros de vista de base de datos — eso es solo de la interfaz web.
- Límite de velocidad: ~3 peticiones/segundo en promedio; las respuestas `429` incluyen `Retry-After`.
- Hasta 100 elementos por petición al añadir bloques hijos.
