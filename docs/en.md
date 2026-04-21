# Skills Catalog — English

> 33 skills available · [Español](es.md)

## Contributing

- Each skill folder must contain a `SKILL.md` with at minimum `name` and `description` in the frontmatter.
- The skill slug (folder name) must be **identical across all languages** — use the canonical English slug.
- Adding a skill in one language without adding it to the others will fail the CI parity check.
- New languages: create a folder under `public/` (e.g. `public/fr/`) and translate all existing skills.

---

## Skill format

Each `SKILL.md` must include a YAML frontmatter block at the top:

````yaml
---
name: my-skill
description: One-line description of what this skill does.
icon: 🔧
category: dev
---

Skill instructions go here...
````

Required fields: `name`, `description`. Optional: `icon`, `category`, `homepage`.

---

## Skills

| Skill | Description |
|-------|-------------|
| [1password](../public/en/1password/SKILL.md) | Set up and use 1Password CLI (op) for installing, signing in, and reading/injecting secrets. |
| [apple-notes](../public/en/apple-notes/SKILL.md) | Manage Apple Notes via the `memo` CLI on macOS (create, view, edit, delete, search, move, export). |
| [apple-reminders](../public/en/apple-reminders/SKILL.md) | Manage Apple Reminders via remindctl CLI (list, add, edit, complete, delete). |
| [bear-notes](../public/en/bear-notes/SKILL.md) | Create, search, and manage Bear notes via grizzly CLI. |
| [blogwatcher](../public/en/blogwatcher/SKILL.md) | Monitor blogs and RSS/Atom feeds for updates using the blogwatcher CLI. |
| [camsnap](../public/en/camsnap/SKILL.md) | Capture frames or clips from RTSP/ONVIF cameras. |
| [coding-agent](../public/en/coding-agent/SKILL.md) | Delegate coding tasks to Codex, Claude Code, or Pi agents via background process. |
| [discord](../public/en/discord/SKILL.md) | Discord operations via the message tool. |
| [gemini](../public/en/gemini/SKILL.md) | Gemini CLI for one-shot Q&A, summaries, and generation. |
| [gh-issues](../public/en/gh-issues/SKILL.md) | Fetch GitHub issues, spawn sub-agents to implement fixes, open PRs, and monitor review comments. |
| [github](../public/en/github/SKILL.md) | GitHub operations via `gh` CLI: issues, PRs, CI runs, code review, API queries. |
| [gog](../public/en/gog/SKILL.md) | Google Workspace CLI for Gmail, Calendar, Drive, Contacts, Sheets, and Docs. |
| [goplaces](../public/en/goplaces/SKILL.md) | Query Google Places API (New) via goplaces CLI for text search, place details, and reviews. |
| [healthcheck](../public/en/healthcheck/SKILL.md) | Host security hardening and risk-tolerance configuration for OpenClaw deployments. |
| [himalaya](../public/en/himalaya/SKILL.md) | CLI to manage emails via IMAP/SMTP: list, read, write, reply, forward, search, and organize. |
| [imsg](../public/en/imsg/SKILL.md) | iMessage/SMS CLI for listing chats, history, and sending messages via Messages.app. |
| [nano-pdf](../public/en/nano-pdf/SKILL.md) | Edit PDFs with natural-language instructions using the nano-pdf CLI. |
| [notion](../public/en/notion/SKILL.md) | Notion API for creating and managing pages, databases, and blocks. |
| [obsidian](../public/en/obsidian/SKILL.md) | Work with Obsidian vaults (plain Markdown notes) and automate via obsidian-cli. |
| [openai-whisper](../public/en/openai-whisper/SKILL.md) | Local speech-to-text with the Whisper CLI (no API key). |
| [openai-whisper-api](../public/en/openai-whisper-api/SKILL.md) | Transcribe audio via OpenAI Audio Transcriptions API (Whisper). |
| [oracle](../public/en/oracle/SKILL.md) | Best practices for using the oracle CLI (prompt + file bundling, engines, sessions). |
| [peekaboo](../public/en/peekaboo/SKILL.md) | Capture and automate macOS UI with the Peekaboo CLI. |
| [sag](../public/en/sag/SKILL.md) | ElevenLabs text-to-speech with mac-style say UX. |
| [slack](../public/en/slack/SKILL.md) | Control Slack from OpenClaw: react to messages, pin/unpin items in channels or DMs. |
| [summarize](../public/en/summarize/SKILL.md) | Summarize or extract text/transcripts from URLs, podcasts, and local files. |
| [things-mac](../public/en/things-mac/SKILL.md) | Manage Things 3 via the `things` CLI on macOS (add/update projects and todos). |
| [tmux](../public/en/tmux/SKILL.md) | Remote-control tmux sessions by sending keystrokes and scraping pane output. |
| [trello](../public/en/trello/SKILL.md) | Manage Trello boards, lists, and cards via the Trello REST API. |
| [video-frames](../public/en/video-frames/SKILL.md) | Extract frames or short clips from videos using ffmpeg. |
| [wacli](../public/en/wacli/SKILL.md) | Send WhatsApp messages and search/sync WhatsApp history via the wacli CLI. |
| [weather](../public/en/weather/SKILL.md) | Get current weather and forecasts via wttr.in or Open-Meteo. No API key needed. |
| [xurl](../public/en/xurl/SKILL.md) | CLI for the X (Twitter) API v2: post, reply, search, DMs, media upload, and more. |
