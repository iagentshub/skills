---
id: NOTION
name: Notion
description: "Notion API for creating and managing pages, databases, and blocks. Requires NOTION_API_KEY. Use to create/read/update pages and data sources (databases) in Notion."
icon: 📝
category: notes
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Notion

Use the Notion API to create/read/update pages, data sources (databases), and blocks.

Homepage: https://developers.notion.com

## Setup

1. Create an integration at https://notion.so/my-integrations
2. Copy the API key (starts with `ntn_` or `secret_`)
3. Save it as an environment variable:

```bash
export NOTION_API_KEY="ntn_your_key_here"
```

4. Share target pages/databases with your integration (click "..." → "Connect to" → your integration)

## Authentication headers

All requests need:

```bash
curl -X GET "https://api.notion.com/v1/..." \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json"
```

> **Note:** The `Notion-Version` header is required. This skill uses `2025-09-03` (latest).

## Common operations

### Search pages and databases

```bash
curl -X POST "https://api.notion.com/v1/search" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{"query": "page title"}'
```

### Get a page

```bash
curl "https://api.notion.com/v1/pages/{page_id}" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03"
```

### Get page content (blocks)

```bash
curl "https://api.notion.com/v1/blocks/{page_id}/children" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03"
```

### Create a page in a database

```bash
curl -X POST "https://api.notion.com/v1/pages" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{
    "parent": {"database_id": "xxx"},
    "properties": {
      "Name": {"title": [{"text": {"content": "New item"}}]},
      "Status": {"select": {"name": "To do"}}
    }
  }'
```

### Query a database

```bash
curl -X POST "https://api.notion.com/v1/data_sources/{data_source_id}/query" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{
    "filter": {"property": "Status", "select": {"equals": "Active"}},
    "sorts": [{"property": "Date", "direction": "descending"}]
  }'
```

### Update page properties

```bash
curl -X PATCH "https://api.notion.com/v1/pages/{page_id}" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{"properties": {"Status": {"select": {"name": "Done"}}}}'
```
