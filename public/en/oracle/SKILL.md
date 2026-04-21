---
id: ORACLE
name: Oracle CLI (file-context query)
description: Bundle a prompt with selected files to make a one-shot query to a large language model with real repository context. Useful for deep analyses that require reading a lot of code.
icon: 🧿
category: dev
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Oracle CLI

Oracle packages your prompt and selected files into a "one-shot" query for another model to answer with real repository context. Treat the response as guidance: always verify against code and tests.

## Requirements

- `oracle` installed

## Installation

```bash
npm install -g @steipete/oracle
# or without installing
npx -y @steipete/oracle --help
```

## Recommended workflow

1. Choose a minimal set of files (those containing the relevant truth).
2. Preview the payload and token cost (`--dry-run` + `--files-report`).
3. Run the query with the appropriate engine and model.
4. If the session disconnects/expires, reconnect to the saved session (don't relaunch).

## Commands

### Help

```bash
oracle --help
npx -y @steipete/oracle --help   # without prior installation
```

### Preview (without spending tokens)

```bash
# Summary without executing
oracle --dry-run summary -p "Describe what this function does" --file "src/**"

# With token report
oracle --dry-run summary --files-report -p "Task" --file "src/**"
```

### Run a query

```bash
# API mode (fast, pay per token)
oracle -p "What does this function do and how can it be improved?" \
  --file "app/backend/engine/runner.py" \
  --file "app/backend/executors/**"

# Render and copy to clipboard (paste manually)
oracle --render --copy -p "Task" --file "src/**"
```

## Attaching files (`--file`)

`--file` accepts files, directories, and globs. Can be used multiple times:

```bash
# Include entire directory
oracle -p "..." --file "app/backend/"

# Multiple paths
oracle -p "..." --file "app/backend/engine/" --file "README.md"

# Exclude files
oracle -p "..." --file "app/**" --file "!app/**/*.pyc" --file "!**/__pycache__/**"
```

**Ignored by default:** `node_modules`, `dist`, `coverage`, `.git`, `build`, `tmp`.
Respects `.gitignore` when expanding globs. Does not follow symlinks.

## Notes

- Choose the minimal set of files containing the needed information; more files = more tokens.
- `--render --copy` mode generates the prompt to clipboard for pasting into any chat.
- Always use `--dry-run` before large queries to estimate cost.
- Treat responses as guidance; always verify in the actual code.
