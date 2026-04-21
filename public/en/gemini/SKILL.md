---
id: GEMINI
name: Gemini CLI
description: Gemini CLI for queries, summaries, and content generation in one-shot mode. Use when you need to query Google's Gemini model directly from the terminal.
icon: ✨
category: ai
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Gemini CLI

Use the Gemini CLI in one-shot mode to answer questions, generate content, and summarize information. Avoid interactive mode.

## Requirements

- `gemini` CLI installed
- Authentication configured (run `gemini` once interactively and follow the login flow)

## Installation

```bash
brew install gemini-cli
```

## Basic usage

```bash
# Simple query
gemini "What is the capital of France?"

# With specific model
gemini --model gemini-2.5-pro "Explain the theory of relativity in 3 paragraphs"

# JSON response
gemini --output-format json "Return a JSON with the 5 most populated capitals in Europe"
```

## Extensions

```bash
# List available extensions
gemini --list-extensions

# Manage extensions
gemini extensions --help
```

## Notes

- First time, run `gemini` interactively to complete the Google login flow.
- Always use one-shot mode (prompt as argument); avoid interactive mode in scripts and agents.
- Avoid `--yolo` for security reasons.
- Full reference: `gemini --help`.
