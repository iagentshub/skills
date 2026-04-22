---
id: APPLE_NOTES
name: Apple Notes
description: "Manage Apple Notes via the memo CLI on macOS: create, view, edit, delete, search, move, and export notes. macOS-only with Notes.app accessible."
icon: 🍎
category: notes
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Apple Notes CLI

Use `memo notes` to manage Apple Notes directly from the terminal.
Create, view, edit, delete, search, move notes between folders, and export to HTML/Markdown.

**macOS only.** Homepage: https://github.com/antoniorodr/memo

## Installation

```bash
# Homebrew (recommended)
brew tap antoniorodr/memo && brew install antoniorodr/memo/memo

# Manual (pip, after cloning the repo)
pip install .
```

If prompted, grant Automation access to Notes.app in:
**System Settings → Privacy & Security → Automation**

## View notes

```bash
# List all notes
memo notes

# Filter by folder
memo notes -f "Folder Name"

# Search notes (fuzzy)
memo notes -s "query"
```

## Create notes

```bash
# New note with interactive editor
memo notes -a

# Quick add with title
memo notes -a "Note Title"
```

## Edit notes

```bash
# Edit note (interactive selection)
memo notes -e
```

## Delete notes

```bash
# Delete note (interactive selection)
memo notes -d
```

## Move notes

```bash
# Move note to folder (interactive selection)
memo notes -m
```

## Export notes

```bash
# Export to HTML/Markdown (interactive selection)
memo notes -ex
```

## Notes

- macOS-only.
- Cannot edit notes containing images or attachments.
- Requires Notes.app accessible; grant Automation permission if prompted.
