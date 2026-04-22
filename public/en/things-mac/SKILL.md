---
id: THINGS_MAC
name: Things 3 Task Management
description: Manage Things 3 on macOS via the things CLI. Read inbox/today/search from the local database and add/update todos via the Things URL scheme.
icon: ✅
category: productivity
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Things 3 Task Management

> ⚠️ **macOS only.** Requires Things 3 installed.

Use `things` to read the local Things database (inbox, today, search, projects, areas, tags) and add/update todos via the Things URL scheme.

## Requirements

- macOS with Things 3 installed
- `things` CLI installed

## Installation

```bash
GOBIN=/opt/homebrew/bin go install github.com/ossianhempel/things3-cli/cmd/things@latest
```

If database reads fail, grant **Full Disk Access** to the app running the CLI (Terminal for manual use; your agent app for automated use).

## Optional config

```bash
# Environment variable for database folder
export THINGSDB=/path/to/ThingsData-xxx

# Token for write operations
export THINGS_AUTH_TOKEN=your-token-here
```

## Read (local database)

```bash
# Inbox
things inbox --limit 50

# Today's tasks
things today

# Upcoming tasks
things upcoming

# Search tasks
things search "dentist"

# Projects, areas, and tags
things projects
things areas
things tags
```

## Write (URL scheme)

```bash
# Preview without executing
things --dry-run add "Buy bread"

# Add basic task
things add "Buy bread"

# With notes
things add "Buy milk" --notes "Skim + juice"

# In a project or area
things add "Book flight" --list "Travel"

# With section inside project
things add "Charge laptop" --list "Travel" --heading "Before leaving"

# With tags
things add "Call doctor" --tags "health,phone"

# With checklist
things add "Pack for trip" --checklist-item "Passport" --checklist-item "Tickets"

# With date and deadline
things add "Submit report" --when today --deadline 2026-05-01
```

## Modify task (requires auth token)

```bash
# Get task UUID
things search "milk" --limit 5

# Change title
things update --id <UUID> --auth-token <TOKEN> "New title"

# Append notes
things update --id <UUID> --auth-token <TOKEN> --append-notes "Extra note"
```
