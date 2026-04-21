---
id: TRELLO
name: Trello
description: "Gestión de tableros, listas y tarjetas de Trello mediante la API REST. Requiere TRELLO_API_KEY y TRELLO_TOKEN. Úsalo para crear, mover, comentar y archivar tarjetas, así como para listar tableros y listas."
icon: 📋
category: productivity
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Trello

Gestiona tableros, listas y tarjetas de Trello directamente con la API REST.

Homepage: https://developer.atlassian.com/cloud/trello/rest/

## Configuración

1. Obtén tu clave de API: https://trello.com/app-key
2. Genera un token (haz clic en el enlace "Token" de esa página)
3. Exporta las variables de entorno:

```bash
export TRELLO_API_KEY="tu-clave-api"
export TRELLO_TOKEN="tu-token"
```

## Operaciones comunes

### Listar tableros

```bash
curl -s "https://api.trello.com/1/members/me/boards?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  | jq '.[] | {name, id}'
```

### Listar listas de un tablero

```bash
curl -s "https://api.trello.com/1/boards/{boardId}/lists?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  | jq '.[] | {name, id}'
```

### Listar tarjetas de una lista

```bash
curl -s "https://api.trello.com/1/lists/{listId}/cards?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  | jq '.[] | {name, id, desc}'
```

### Crear una tarjeta

```bash
curl -s -X POST "https://api.trello.com/1/cards?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  -d "idList={listId}" \
  -d "name=Título de la tarjeta" \
  -d "desc=Descripción de la tarjeta"
```

### Mover una tarjeta a otra lista

```bash
curl -s -X PUT "https://api.trello.com/1/cards/{cardId}?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  -d "idList={newListId}"
```

### Añadir un comentario a una tarjeta

```bash
curl -s -X POST "https://api.trello.com/1/cards/{cardId}/actions/comments?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  -d "text=Tu comentario aquí"
```

### Archivar una tarjeta

```bash
curl -s -X PUT "https://api.trello.com/1/cards/{cardId}?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  -d "closed=true"
```

## Ejemplos

```bash
# Obtener todos los tableros
curl -s "https://api.trello.com/1/members/me/boards?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN&fields=name,id" | jq

# Encontrar un tablero por nombre
curl -s "https://api.trello.com/1/members/me/boards?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  | jq '.[] | select(.name | contains("Trabajo"))'

# Obtener todas las tarjetas de un tablero
curl -s "https://api.trello.com/1/boards/{boardId}/cards?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  | jq '.[] | {name, list: .idList}'
```

## Notas

- Los IDs de tablero/lista/tarjeta se encuentran en la URL de Trello o mediante los comandos de listado.
- La clave de API y el token dan acceso completo a tu cuenta de Trello — ¡mantenlos en secreto!
- Límites de velocidad: 300 peticiones/10 segundos por clave de API; 100 peticiones/10 segundos por token.
