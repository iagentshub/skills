---
id: CODING_AGENT
name: Coding Agent
description: "Delegate coding tasks to Claude Code or Codex via background process. Use when: (1) building new features, (2) reviewing PRs in temp directory, (3) refactoring large codebases, (4) iterative file exploration. NOT for: one-liner fixes (edit directly), reading code (use read tool), work in own agent workspace."
icon: 🧩
category: dev
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Coding Agent

Use **bash** (with optional background mode) for all coding agent work.

## PTY: Codex yes, Claude Code no

For **Codex**, PTY is still required (interactive terminal app):

```bash
# ✅ Correct for Codex
bash pty:true command:"codex exec 'Your task here'"
```

For **Claude Code** (`claude` CLI), use `--print --permission-mode bypassPermissions`:

```bash
# ✅ Correct for Claude Code (no PTY)
cd /path/to/project && claude --permission-mode bypassPermissions --print 'Your task'

# For background execution:
bash workdir:~/project background:true command:"claude --permission-mode bypassPermissions --print 'Your task'"
```

## Quick start: single-step tasks

```bash
# Quick chat (Codex needs a git repo!)
SCRATCH=$(mktemp -d) && cd $SCRATCH && git init && codex exec "Your task here"

# In a real project — with PTY!
bash pty:true workdir:~/Projects/my-project command:"codex exec 'Add error handling to API calls'"
```

**Why git init?** Codex refuses to run outside a trusted git directory.

## The pattern: workdir + background + pty

For longer tasks, use background mode with PTY:

```bash
# Start agent in target directory (with PTY!)
bash pty:true workdir:~/project background:true command:"codex exec --full-auto 'Build a snake game'"
# Returns sessionId for tracking

# Monitor progress
process action:log sessionId:XXX

# Check if done
process action:poll sessionId:XXX

# Send input (if agent asks a question)
process action:submit sessionId:XXX data:"yes"

# Kill if needed
process action:kill sessionId:XXX
```

## Codex CLI flags

| Flag | Effect |
|------|--------|
| `exec "task"` | One-shot execution, exits when done |
| `--full-auto` | Sandboxed, auto-approves in workspace |
| `--yolo` | NO sandbox, NO approvals (fastest, most dangerous) |

## Claude Code

```bash
# Foreground
bash workdir:~/project command:"claude --permission-mode bypassPermissions --print 'Your task'"

# Background
bash workdir:~/project background:true command:"claude --permission-mode bypassPermissions --print 'Your task'"
```

## Reviewing PRs

⚠️ Never review PRs in the agent's own project folder. Clone to a temp folder.

```bash
REVIEW_DIR=$(mktemp -d)
git clone https://github.com/user/repo.git $REVIEW_DIR
cd $REVIEW_DIR && gh pr checkout 130
bash pty:true workdir:$REVIEW_DIR command:"codex review --base origin/main"
```

## Rules

1. Use the right mode: Codex/Pi/OpenCode → `pty:true`; Claude Code → `--print --permission-mode bypassPermissions`.
2. Respect tool choice — if user asks for Codex, use Codex.
3. Be patient — don't kill sessions because they're slow.
4. Monitor with `process:log` without interfering.
5. Never start Codex inside your own agent state directory.
