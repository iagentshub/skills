---
id: BEAR_NOTES
name: Bear Notes (grizzly)
description: Create, search, and manage Bear notes on macOS via the grizzly CLI. Allows creating notes with tags, reading by ID, appending text, and listing tags.
icon: 🐻
category: notes
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Bear Notes (grizzly)

> ⚠️ **macOS only.** Requires Bear app installed and running.

Use `grizzly` to create, read, and manage notes in Bear.

## Installation

```bash
go install github.com/tylerwince/grizzly/cmd/grizzly@latest
```

## Bear token (for some operations)

Some operations (add-text, list tags, open selected note) require an authentication token:

1. Open Bear → Help → API Token → Copy Token
2. Save it: `echo "YOUR_TOKEN" > ~/.config/grizzly/token`

## Common commands

### Create note

```bash
# From stdin
echo "Note content" | grizzly create --title "My Note" --tag work

# Empty note
grizzly create --title "Quick Note" --tag inbox < /dev/null
```

### Open/read note by ID

```bash
grizzly open-note --id "NOTE_ID" --enable-callback --json
```

### Append text to a note

```bash
echo "Additional content" | grizzly add-text --id "NOTE_ID" \
  --mode append --token-file ~/.config/grizzly/token
```

### List all tags

```bash
grizzly tags --enable-callback --json --token-file ~/.config/grizzly/token
```

### Search notes by tag

```bash
grizzly open-tag --name "work" --enable-callback --json
```

## Common flags

- `--dry-run` — Preview URL without executing
- `--print-url` — Show the x-callback-url
- `--enable-callback` — Wait for Bear's response (required for reading data)
- `--json` — Output as JSON (when using callbacks)
- `--token-file PATH` — Path to Bear API token file

## Notes

- Bear must be running for commands to work.
- Note IDs are Bear's internal identifiers (visible in note info or via callbacks).
- Use `--enable-callback` when you need to read data back from Bear.
