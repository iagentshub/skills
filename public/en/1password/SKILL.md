---
id: 1PASSWORD
name: 1Password
description: "Set up and use the 1Password CLI (op). Use to install the CLI, enable desktop app integration, sign in (single or multi-account), and read/inject secrets with op. Only if you use 1Password as your password manager."
icon: 🔐
category: security
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# 1Password CLI

Follow the official get-started steps. Don't guess install commands.

Homepage: https://developer.1password.com/docs/cli/get-started/

## Workflow

1. Check OS and shell.
2. Verify CLI is installed: `op --version`.
3. Confirm desktop app integration is enabled and the app is unlocked.
4. **REQUIRED:** create a tmux session for all `op` commands (never call `op` outside tmux).
5. Sign in / authorize inside tmux: `op signin` (wait for app confirmation).
6. Verify access inside tmux: `op whoami` (must succeed before reading any secrets).
7. Multiple accounts: use `--account` or `OP_ACCOUNT`.

## Installation

```bash
brew install 1password-cli
```

## Required tmux session

The shell tool uses a fresh TTY per command. To avoid re-prompts and failures, always run `op` inside a dedicated tmux session:

```bash
SESSION="op-auth-$(date +%Y%m%d-%H%M%S)"
tmux new-session -d -s "$SESSION"
tmux send-keys -t "$SESSION" "op signin" Enter
tmux send-keys -t "$SESSION" "op whoami" Enter
tmux send-keys -t "$SESSION" "op vault list" Enter
tmux capture-pane -p -J -t "$SESSION" -S -200
tmux kill-session -t "$SESSION"
```

## Common commands

```bash
# Verify active session
op whoami

# List vaults
op vault list

# Read a secret
op read "op://vault/item/field"

# Inject secrets into a command
op run -- your-command-with-env-vars

# Inject secrets into a template file
op inject -i template.env -o .env
```

## Security guards

- Never paste secrets into logs, chat, or code.
- Prefer `op run` / `op inject` over writing secrets to disk.
- If sign-in without app integration is needed, use `op account add`.
- If a command returns "account is not signed in", re-run `op signin` inside tmux and authorize in the app.
- Do not run `op` outside tmux.
