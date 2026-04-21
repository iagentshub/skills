---
id: IMESSAGE
name: iMessage / SMS (imsg)
description: Read and send iMessage/SMS on macOS using the imsg CLI via Messages.app. macOS only with Messages signed into an Apple ID account.
icon: 📨
category: messaging
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: iMessage / SMS (imsg)

> ⚠️ **macOS only.** Requires Messages.app signed in with an Apple ID.

Use `imsg` to read and send iMessage/SMS via the macOS Messages app.

## When to use

✅ **USE this skill when:**

- User explicitly asks to send an iMessage or SMS
- Reading iMessage conversation history
- Checking recent chats in Messages.app
- Sending to phone numbers or Apple IDs

## When NOT to use

❌ **DON'T use this skill when:**

- Telegram, Signal, WhatsApp, or Discord messages → use their respective channels
- Group management (add/remove members) → not supported
- Bulk messaging → always confirm with the user first
- User is chatting with you via iMessage → reply normally

## Requirements

- macOS with Messages.app signed in
- Full Disk Access for the terminal
- Automation permission for Messages.app (for sending)

## Installation

```bash
brew install steipete/tap/imsg
```

## Commands

### List chats

```bash
imsg chats --limit 10 --json
```

### View conversation history

```bash
# By chat ID
imsg history --chat-id 1 --limit 20 --json

# With attachment info
imsg history --chat-id 1 --limit 20 --attachments --json
```

### Send message

```bash
# To phone number
imsg send --to "+15551234567" --message "Hello, are you available?"

# To Apple ID (email)
imsg send --to "person@icloud.com" --message "Sending the documents now"
```

### Monitor new messages

```bash
imsg watch --chat-id 1 --attachments
```

## Notes

- Get chat ID with `imsg chats --limit 20`.
- Messages are sent via Messages.app (not directly to Apple's network).
- Always confirm recipient and message before sending.
- macOS only; does not work on Linux or Windows.
