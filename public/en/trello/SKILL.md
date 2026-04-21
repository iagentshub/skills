---
id: TRELLO
name: Trello
description: "Manage Trello boards, lists, and cards via the REST API. Requires TRELLO_API_KEY and TRELLO_TOKEN. Use to create, move, comment, and archive cards, and to list boards and lists."
icon: 📋
category: productivity
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Trello

Manage Trello boards, lists, and cards directly with the REST API.

Homepage: https://developer.atlassian.com/cloud/trello/rest/

## Setup

1. Get your API key: https://trello.com/app-key
2. Generate a token (click the "Token" link on that page)
3. Export the environment variables:

```bash
export TRELLO_API_KEY="your-api-key"
export TRELLO_TOKEN="your-token"
```

## Common operations

### List boards

```bash
curl -s "https://api.trello.com/1/members/me/boards?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  | jq '.[] | {name, id}'
```

### List lists in a board

```bash
curl -s "https://api.trello.com/1/boards/{boardId}/lists?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  | jq '.[] | {name, id}'
```

### List cards in a list

```bash
curl -s "https://api.trello.com/1/lists/{listId}/cards?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  | jq '.[] | {name, id, desc}'
```

### Create a card

```bash
curl -s -X POST "https://api.trello.com/1/cards?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  -d "idList={listId}" \
  -d "name=Card title" \
  -d "desc=Card description"
```

### Move a card to another list

```bash
curl -s -X PUT "https://api.trello.com/1/cards/{cardId}?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  -d "idList={newListId}"
```

### Add a comment to a card

```bash
curl -s -X POST "https://api.trello.com/1/cards/{cardId}/actions/comments?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  -d "text=Your comment here"
```

### Archive a card

```bash
curl -s -X PUT "https://api.trello.com/1/cards/{cardId}?key=$TRELLO_API_KEY&token=$TRELLO_TOKEN" \
  -d "closed=true"
```

## Notes

- Board/list/card IDs can be found in the Trello URL or via the listing commands.
- API key and token give full access to your Trello account — keep them secret!
- Rate limits: 300 requests/10s per API key; 100 requests/10s per token.
