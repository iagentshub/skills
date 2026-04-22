---
id: GOOGLE_WORKSPACE
name: Google Workspace CLI (gog)
description: CLI for Gmail, Calendar, Drive, Contacts, Sheets, and Google Docs. Use when the user asks to manage Gmail, Google Calendar events, Drive files, Sheets, or Docs.
icon: 📬
category: productivity
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Google Workspace CLI (gog)

`gog` is a modern CLI to interact with Gmail, Calendar, Drive, Contacts, Sheets, and Docs. Requires OAuth authentication.

## Requirements

- `gog` installed (`brew install steipete/tap/gogcli`)
- Google OAuth credentials configured

## Initial setup (once)

```bash
gog auth credentials /path/client_secret.json
gog auth add you@gmail.com --services gmail,calendar,drive,contacts,docs,sheets
gog auth list
```

## Gmail

```bash
# Search emails (last 7 days)
gog gmail search 'newer_than:7d' --max 10

# Search individual messages (no thread grouping)
gog gmail messages search "in:inbox from:example.com" --max 20

# Send plain text
gog gmail send --to recipient@example.com --subject "Hello" --body "Message here"

# Send from file (supports multiple paragraphs)
gog gmail send --to recipient@example.com --subject "Hello" --body-file ./message.txt

# Send from stdin
gog gmail send --to recipient@example.com --subject "Hello" --body-file - <<'EOF'
Hello,

This is the message body.
It can have multiple lines.

Regards
EOF

# Send HTML
gog gmail send --to recipient@example.com --subject "Hello" --body-html "<p>Hello</p>"

# Create draft
gog gmail drafts create --to dest@example.com --subject "Draft" --body-file ./draft.txt

# Send draft
gog gmail drafts send <draftId>

# Reply to a message
gog gmail send --to dest@example.com --subject "Re: Subject" --body "Reply" \
  --reply-to-message-id <msgId>
```

## Calendar

```bash
# List events
gog calendar events <calendarId> --from 2026-04-18T00:00:00Z --to 2026-04-25T23:59:59Z

# Create event
gog calendar create <calendarId> --summary "Meeting" \
  --from 2026-04-20T10:00:00Z --to 2026-04-20T11:00:00Z

# Create event with color (1-11)
gog calendar create <calendarId> --summary "Meeting" \
  --from 2026-04-20T10:00:00Z --to 2026-04-20T11:00:00Z --event-color 7

# View available colors
gog calendar colors

# Update event
gog calendar update <calendarId> <eventId> --summary "New title" --event-color 4
```

**Event color IDs (1–11):**
`1`=#a4bdfc · `2`=#7ae7bf · `3`=#dbadff · `4`=#ff887c · `5`=#fbd75b · `6`=#ffb878 · `7`=#46d6db · `8`=#e1e1e1 · `9`=#5484ed · `10`=#51b749 · `11`=#dc2127

## Drive

```bash
gog drive search "report" --max 10
gog drive list
```

## Contacts

```bash
gog contacts search "John"
gog contacts list --max 20
```

## Sheets

```bash
# Read a range
gog sheets read <spreadsheetId> "Sheet1!A1:D10"

# Write values
gog sheets write <spreadsheetId> "Sheet1!A1" --values '[["Name","Age"],["Alice","30"]]'
```
