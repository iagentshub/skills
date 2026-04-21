---
id: TWITTER_X
name: Twitter / X API
description: CLI to interact with the X (Twitter) v2 API. Post tweets, reply, quote, search, manage followers, send DMs, and upload media using xurl.
icon: 🐦
category: messaging
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Twitter / X API (xurl)

`xurl` is a CLI for the X API. Supports agent-friendly shortcut commands and direct access to any v2 endpoint. All responses are JSON.

## Requirements

- `xurl` installed
- OAuth authentication configured (`xurl auth status` to verify)

## Installation

```bash
brew install --cask xdevplatform/tap/xurl
# or
npm install -g @xdevplatform/xurl
```

## Security (required)

- **Never** read, print, or send the `~/.xurl` file to agent context.
- **Never** ask the user to paste credentials/tokens in chat.
- **Never** use `--verbose`/`-v` in agent sessions (may expose tokens in output).
- Flags that **must never** be used in agent commands: `--bearer-token`, `--consumer-key`, `--consumer-secret`, `--access-token`, `--token-secret`, `--client-id`, `--client-secret`.
- App registration and initial authentication must be done manually by the user, outside the agent session.

## Authentication

```bash
# Verify authentication
xurl auth status

# Authenticate (user does this manually)
xurl auth oauth2
```

## Quick reference

| Action | Command |
|--------|---------|
| Post tweet | `xurl post "Hello world!"` |
| Reply | `xurl reply POST_ID "Reply"` |
| Quote | `xurl quote POST_ID "My take"` |
| Delete tweet | `xurl delete POST_ID` |
| Read tweet | `xurl read POST_ID` |
| Search tweets | `xurl search "QUERY" -n 10` |
| Who am I | `xurl whoami` |
| View user | `xurl user @handle` |
| Own timeline | `xurl timeline -n 20` |
| Mentions | `xurl mentions -n 10` |
| Like | `xurl like POST_ID` |
| Unlike | `xurl unlike POST_ID` |
| Repost | `xurl repost POST_ID` |
| Bookmark | `xurl bookmark POST_ID` |
| View bookmarks | `xurl bookmarks -n 10` |
| Follow | `xurl follow @handle` |
| Unfollow | `xurl unfollow @handle` |
| View followers | `xurl followers -n 20` |
| View following | `xurl following -n 20` |
| Block | `xurl block @handle` |
| Mute | `xurl mute @handle` |
| Send DM | `xurl dm @handle "message"` |
| View DMs | `xurl dms -n 10` |
| Upload media | `xurl media upload /path/to/file.mp4` |
| Media status | `xurl media status MEDIA_ID` |

## Raw API access

```bash
# Any v2 endpoint
xurl GET /2/users/me
xurl GET /2/tweets?ids=1234567890
```

## Multiple apps

```bash
# List registered apps
xurl auth apps list

# Set default app
xurl auth default my-app

# Use specific app for a command
xurl --app my-app /2/users/me
```

## Notes

- All commands return JSON to stdout.
- Use `-n` to limit results and avoid unnecessary requests.
- Always confirm before posting tweets or sending DMs.
