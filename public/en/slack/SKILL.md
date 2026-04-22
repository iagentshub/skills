---
id: SLACK
name: Slack
description: "Slack operations: send/edit/delete messages, react, pin messages, read conversations, and get member info. Requires a Slack bot token configured as a channel."
icon: 💬
category: messaging
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Slack

Use Slack actions to react, manage pinned messages, send/edit/delete messages, and get member information.

## Data to collect

- `channelId` and `messageId` (Slack message timestamp, e.g. `1712023032.1234`).
- For reactions, an `emoji` (Unicode or `:name:`).
- For message sends, a `to` destination (`channel:<id>` or `user:<id>`) and `content`.

## Action groups

| Group | Default | Notes |
|-------|---------|-------|
| reactions | active | React + list reactions |
| messages | active | Read/send/edit/delete |
| pins | active | Pin/unpin/list |
| memberInfo | active | Member info |
| emojiList | active | Custom emoji list |

## Common actions

### React to a message

```json
{
  "action": "react",
  "channelId": "C123",
  "messageId": "1712023032.1234",
  "emoji": "✅"
}
```

### List reactions

```json
{
  "action": "reactions",
  "channelId": "C123",
  "messageId": "1712023032.1234"
}
```

### Send a message

```json
{
  "action": "sendMessage",
  "to": "channel:C123",
  "content": "Hello from the agent"
}
```

### Edit a message

```json
{
  "action": "editMessage",
  "channelId": "C123",
  "messageId": "1712023032.1234",
  "content": "Updated text"
}
```

### Delete a message

```json
{
  "action": "deleteMessage",
  "channelId": "C123",
  "messageId": "1712023032.1234"
}
```

### Read recent messages

```json
{
  "action": "readMessages",
  "channelId": "C123",
  "limit": 20
}
```

### Pin a message

```json
{
  "action": "pinMessage",
  "channelId": "C123",
  "messageId": "1712023032.1234"
}
```

### Unpin a message

```json
{
  "action": "unpinMessage",
  "channelId": "C123",
  "messageId": "1712023032.1234"
}
```

### List pinned messages

```json
{
  "action": "listPins",
  "channelId": "C123"
}
```

### Get member info

```json
{
  "action": "memberInfo",
  "userId": "U123"
}
```
