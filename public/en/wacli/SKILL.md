---
id: WHATSAPP
name: WhatsApp CLI (wacli)
description: Send WhatsApp messages to third parties and search/sync WhatsApp history using the wacli CLI. ONLY for messages to third parties, not for the agent's chat channel with the user.
icon: 📱
category: messaging
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: WhatsApp CLI (wacli)

Use `wacli` **only** when the user explicitly asks to send a message to another person via WhatsApp, or asks to sync/search WhatsApp history.

> ⚠️ **Important:** This skill is for messages to third parties. If the user is chatting with you via WhatsApp, reply through the normal channel; do not use this tool unless they ask to contact someone else.

## Requirements

- `wacli` installed
- QR authentication completed (`wacli auth`)

## Installation

```bash
brew install steipete/tap/wacli
# or
go install github.com/steipete/wacli/cmd/wacli@latest
```

## Security

- Requires explicit recipient and message text.
- Always confirm recipient and message before sending.
- If there is any ambiguity, ask before acting.

## Authentication and sync

```bash
# Initial login (scan QR with phone)
wacli auth

# Continuous sync
wacli sync --follow

# Diagnostics
wacli doctor
```

## Search chats and messages

```bash
# List chats
wacli chats list --limit 20 --query "name or number"

# Search messages
wacli messages search "query" --limit 20 --chat <jid>

# Search by date range
wacli messages search "invoice" --after 2025-01-01 --before 2025-12-31
```

## Backfill history

```bash
wacli history backfill --chat <jid> --requests 2 --count 50
```

## Send messages

```bash
# Text to phone number
wacli send text --to "+15551234567" --message "Hi, can you talk at 3pm?"

# Message to group
wacli send text --to "1234567890-123456789@g.us" --message "Running 5 minutes late."

# Send file with caption
wacli send file --to "+15551234567" --file /path/agenda.pdf --caption "Agenda"
```
