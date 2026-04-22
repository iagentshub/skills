---
id: SUMMARIZE
name: Summarize
description: "Summarize or extract text and transcriptions from URLs, podcasts, YouTube, and local files. Ideal as a substitute for 'transcribe this video'. Requires the summarize CLI and an LLM provider API key."
icon: 🧾
category: ai
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Summarize

Fast CLI for summarizing URLs, local files, and YouTube videos.

Homepage: https://summarize.sh

## When to use (trigger phrases)

Use this skill immediately when the user asks something like:

- "What is this link/video about?"
- "Summarize this URL/article"
- "Transcribe this YouTube video"
- "What does this PDF say?"

## Quick start

```bash
summarize "https://example.com" --model google/gemini-3-flash-preview
summarize "/path/to/file.pdf" --model google/gemini-3-flash-preview
summarize "https://youtu.be/dQw4w9WgXcQ" --youtube auto
```

## YouTube: summary vs transcription

Direct transcription (URLs only):

```bash
summarize "https://youtu.be/dQw4w9WgXcQ" --youtube auto --extract-only
```

If the user asks for a transcription but it's very long, return a compact summary first and ask which section or time range to expand.

## Installation

```bash
brew tap steipete/tap
brew install steipete/tap/summarize
```

## Models and API keys

Set the key for the provider you'll use:

- OpenAI: `OPENAI_API_KEY`
- Anthropic: `ANTHROPIC_API_KEY`
- xAI: `XAI_API_KEY`
- Google: `GEMINI_API_KEY`

Default model is `google/gemini-3-flash-preview` if none is specified.

## Useful flags

| Flag | Description |
|------|-------------|
| `--length short\|medium\|long\|xl` | Summary length |
| `--max-output-tokens <n>` | Output token limit |
| `--extract-only` | Extract text only (URLs only) |
| `--json` | JSON output |
| `--firecrawl auto\|off\|always` | Alternative extraction for blocked sites |
| `--youtube auto` | Fallback via Apify if you have `APIFY_API_TOKEN` |

## Optional config

Config file: `~/.summarize/config.json`

```json
{ "model": "openai/gpt-5.2" }
```
