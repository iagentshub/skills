---
id: EMAIL
name: Email with Himalaya CLI
description: Manage emails via IMAP/SMTP with the himalaya CLI. List, read, compose, reply, forward, search, and organize emails from the terminal. Supports multiple accounts.
icon: 📧
category: messaging
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Email with Himalaya CLI

`himalaya` is a CLI email client that manages emails from the terminal using IMAP, SMTP, Notmuch, or Sendmail backends.

## Requirements

1. `himalaya` installed (`himalaya --version` to verify)
2. Config file at `~/.config/himalaya/config.toml`
3. IMAP/SMTP credentials configured

## Installation

```bash
brew install himalaya
```

## Configuration

Run the interactive wizard to configure an account:

```bash
himalaya account configure
```

Or create `~/.config/himalaya/config.toml` manually:

```toml
[accounts.personal]
email = "you@example.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.example.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "you@example.com"
backend.auth.type = "password"
backend.auth.cmd = "pass show email/imap"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.example.com"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "you@example.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "pass show email/smtp"
```

## Common operations

### List folders

```bash
himalaya folder list
```

### List emails

```bash
# Inbox (default)
himalaya envelope list

# Specific folder
himalaya envelope list --folder "Sent"

# With pagination
himalaya envelope list --page 1 --page-size 20
```

### Search emails

```bash
himalaya envelope list from john@example.com subject meeting
```

### Read an email

```bash
# Read by ID
himalaya message read 42

# Export full MIME
himalaya message export 42 --full
```

### Reply to an email

```bash
# Interactive reply (opens $EDITOR)
himalaya message reply 42

# Reply to all
himalaya message reply 42 --all
```

### Forward an email

```bash
himalaya message forward 42
```

### Compose a new email

```bash
# Interactive composition (opens $EDITOR)
himalaya message write
```

### Direct send with template

```bash
himalaya message send < template.eml
```

## Notes

- Use `himalaya account list` to see configured accounts.
- Switch accounts with `--account NAME` flag.
- Never expose passwords in config; use `cmd =` to fetch from a password manager.
