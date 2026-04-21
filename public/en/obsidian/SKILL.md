---
id: OBSIDIAN
name: Obsidian
description: "Work with Obsidian vaults (plain Markdown notes) and automate operations with obsidian-cli: search, create, move, rename, and delete notes. macOS/Linux with Obsidian installed."
icon: 💎
category: notes
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Obsidian

An Obsidian vault is simply a folder on disk with Markdown files.

Homepage: https://help.obsidian.md

## Vault structure (typical)

- Notes: `*.md` (plain Markdown; editable with any editor)
- Config: `.obsidian/` (workspace settings and plugins; normally don't touch)
- Canvases: `*.canvas` (JSON)
- Attachments: the folder you configured in Obsidian (images, PDFs, etc.)

## Installation

```bash
brew install yakitrak/yakitrak/obsidian-cli
```

## Find the active vault

Obsidian stores vaults in:
- macOS: `~/Library/Application Support/obsidian/obsidian.json`

`obsidian-cli` resolves vaults from that file; the vault name is normally the folder name.

```bash
# If you already have a default vault configured:
obsidian-cli print-default --path-only

# Otherwise, read the config file:
cat ~/Library/Application\ Support/obsidian/obsidian.json
```

> Don't hardcode vault paths; always read the config or use `print-default`.

## obsidian-cli: quick start

### Set default vault (once)

```bash
obsidian-cli set-default "my-vault-name"
obsidian-cli print-default
```

### Search notes

```bash
# Search by name
obsidian-cli search "query"

# Search by content (shows snippets and line numbers)
obsidian-cli search-content "query"
```

### Create notes

```bash
obsidian-cli create "Folder/New note" --content "Initial content" --open
```

> Requires Obsidian's URI handler (`obsidian://...`) to work (Obsidian installed).
> Avoid creating notes under hidden folders (e.g. `.something/...`); Obsidian may reject them.

### Move/rename (safe refactoring)

```bash
obsidian-cli move "old/path/note" "new/path/note"
```

Updates `[[wikilinks]]` and Markdown links across the vault — this is the main advantage over `mv`.

### Delete notes

```bash
obsidian-cli delete "path/note"
```

## Notes

- Multiple vaults are common (iCloud vs `~/Documents`, work/personal, etc.). Don't guess; read the config.
- For direct edits, open the `.md` file and change it; Obsidian will detect it automatically.
- Avoid hardcoding vault paths in scripts; prefer reading the config or using `print-default`.
