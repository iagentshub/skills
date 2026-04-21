---
id: TMUX
name: tmux Session Control
description: Control tmux sessions by sending keys and reading pane output. Useful for managing long-running interactive processes, interactive CLIs, and background tasks.
icon: 🧵
category: dev
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: tmux Session Control

Control tmux sessions by sending keystrokes and reading output. Essential for managing interactive processes and background tasks.

## When to use

✅ **USE this skill when:**

- Monitoring interactive processes in tmux
- Sending input to interactive terminal applications
- Reading output from long-running processes in tmux
- Navigating tmux panes/windows programmatically
- Checking status of background jobs

## When NOT to use

❌ **DON'T use this skill when:**

- Running one-off shell commands → use the executor directly
- Starting new background processes → use the executor with background mode
- Non-interactive scripts → use the shell executor
- The process is not inside tmux

## Installation

```bash
brew install tmux
```

## Common commands

### List sessions

```bash
tmux list-sessions
tmux ls
```

### Capture output

```bash
# Last 20 lines of the pane
tmux capture-pane -t my-session -p | tail -20

# Full scrollback
tmux capture-pane -t my-session -p -S -

# Specific pane in a window
tmux capture-pane -t my-session:0.0 -p
```

### Send keys

```bash
# Send text (without pressing Enter)
tmux send-keys -t my-session "hello"

# Send text + Enter
tmux send-keys -t my-session "ls -la" Enter

# Special keys
tmux send-keys -t my-session Enter
tmux send-keys -t my-session Escape
tmux send-keys -t my-session C-c          # Ctrl+C
tmux send-keys -t my-session C-d          # Ctrl+D (EOF)
tmux send-keys -t my-session C-z          # Ctrl+Z (suspend)
```

### Window and pane navigation

```bash
# Select window
tmux select-window -t my-session:0

# Select pane
tmux select-pane -t my-session:0.1

# List windows
tmux list-windows -t my-session
```

### Session management

```bash
# Create new session in background
tmux new-session -d -s new-session

# Kill session
tmux kill-session -t my-session
```
