---
id: DISCORD
name: Discord
description: "Discord operations: send/edit/delete messages, react, pin, read channels, manage roles, and moderate. Requires a Discord bot token configured as a channel."
icon: 🎮
category: messaging
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Discord

Discord operations via the messaging tool with `channel: "discord"`.

## Key rules

- Always use `channel: "discord"`.
- Prefer explicit IDs: `guildId`, `channelId`, `messageId`, `userId`.
- Avoid Markdown tables in outbound Discord messages.
- Mention users as `<@USER_ID>`.
- Prefer Discord components v2 (`components`) for rich UI; use `embeds` (legacy) only when necessary.
- Multi-account: use optional `accountId`.

## Targets

- Send-like actions: `to: "channel:<id>"` or `to: "user:<id>"`.
- Message-specific actions: `channelId: "<id>"` + `messageId: "<id>"`.

## Common actions

### Send a message

```json
{
  "action": "send",
  "channel": "discord",
  "to": "channel:123",
  "message": "Hello from the agent",
  "silent": true
}
```

### Send with attachment

```json
{
  "action": "send",
  "channel": "discord",
  "to": "channel:123",
  "message": "See attachment",
  "media": "file:///tmp/example.png"
}
```

### Send with components v2 (recommended for rich UI)

```json
{
  "action": "send",
  "channel": "discord",
  "to": "channel:123",
  "message": "Status update",
  "components": "[Carbon v2 components]"
}
```

> Do not combine `components` with `embeds` (Discord rejects them together).

### React to a message

```json
{
  "action": "react",
  "channel": "discord",
  "channelId": "123",
  "messageId": "456",
  "emoji": "✅"
}
```

### Read recent messages

```json
{
  "action": "read",
  "channel": "discord",
  "channelId": "123",
  "limit": 20
}
```

### Edit a message

```json
{
  "action": "edit",
  "channel": "discord",
  "channelId": "123",
  "messageId": "456",
  "message": "Updated text"
}
```

### Delete a message

```json
{
  "action": "delete",
  "channel": "discord",
  "channelId": "123",
  "messageId": "456"
}
```

### Pin a message

```json
{
  "action": "pin",
  "channel": "discord",
  "channelId": "123",
  "messageId": "456"
}
```

### Create a thread

```json
{
  "action": "thread-create",
  "channel": "discord",
  "channelId": "123",
  "messageId": "456",
  "threadName": "bug triage"
}
```

### Search

```json
{
  "action": "search",
  "channel": "discord",
  "guildId": "999",
  "query": "release notes",
  "channelIds": ["123", "456"],
  "limit": 10
}
```
