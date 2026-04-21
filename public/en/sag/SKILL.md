---
id: TEXT_TO_SPEECH
name: Text to Speech with ElevenLabs (sag)
description: Convert text to speech with ElevenLabs using the sag CLI, with UX similar to macOS say command. Multiple voices, models, and expressive audio tags. Requires ELEVENLABS_API_KEY.
icon: 🔊
category: media
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Text to Speech with ElevenLabs (sag)

Use `sag` for speech synthesis with ElevenLabs and local playback.

## Requirements

- `sag` installed
- `ELEVENLABS_API_KEY` or `SAG_API_KEY` configured

## Installation

```bash
brew install steipete/tap/sag
```

## Basic usage

```bash
# Simple text (default voice)
sag "Hello, this is a test"

# With specific voice
sag speak -v "Roger" "Hello, I'm Roger"

# List available voices
sag voices

# View pronunciation tips per model
sag prompting
```

## Available models

| Model | Description |
|-------|-------------|
| `eleven_v3` | Expressive (default) |
| `eleven_multilingual_v2` | Stable and multilingual |
| `eleven_flash_v2_5` | Fast and economical |

```bash
# Specify model
sag speak --model eleven_multilingual_v2 "Text in English"
```

## Expressive audio tags (v3 only)

Place at the start of a line:

```bash
sag "[whispers] this is a secret. [short pause] understood?"
sag "[excited] We won the championship!"
sag "[sarcastic] sure, great idea..."
```

Available tags: `[whispers]`, `[shouts]`, `[sings]`, `[laughs]`, `[chuckles]`, `[sighs]`, `[exhales]`, `[sarcastic]`, `[curious]`, `[excited]`, `[crying]`, `[with malice]`

Pauses (v3): `[pause]`, `[short pause]`, `[long pause]`
Pauses (v2/v2.5): SSML `<break time="1.5s" />`

## Pronunciation and settings

```bash
# Automatic number/unit normalization
sag --normalize auto "It's 3.5 km or 3500 meters"

# Fix language for normalization
sag --lang en "That costs $1,500 per unit"
```

## Save to file

```bash
# Save without playing
sag speak --output /tmp/output.mp3 "Text to save"
```

## Notes

- `ELEVENLABS_API_KEY` can also be set as `SAG_API_KEY`.
- The default model is `eleven_v3`; use `eleven_multilingual_v2` for better stability.
- Run `sag --help` for the full reference.
