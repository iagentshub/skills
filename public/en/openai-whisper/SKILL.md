---
id: LOCAL_TRANSCRIPTION
name: Local Audio Transcription (Whisper)
description: Transcribe audio locally with the Whisper CLI (no API key required). Supports multiple audio formats and models of different sizes to balance speed and accuracy.
icon: 🎤
category: media
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Local Audio Transcription (Whisper)

Use `whisper` to transcribe audio **locally** without an API key or internet connection (after the first model download).

## Requirements

- `whisper` installed (`pip install openai-whisper` or `brew install openai-whisper`)
- `ffmpeg` on PATH to convert audio formats

## Installation

```bash
brew install openai-whisper
# or
pip install openai-whisper
```

## Basic usage

```bash
# Simple transcription to text
whisper /path/to/audio.mp3 --model medium --output_format txt --output_dir .

# Translate to English
whisper /path/to/audio.m4a --task translate --output_format srt

# Specify language for better accuracy
whisper /path/to/audio.mp3 --language en --model medium
```

## Available models

| Model | Size | Speed | Accuracy |
|-------|------|-------|----------|
| `tiny` | 75 MB | Very fast | Basic |
| `base` | 142 MB | Fast | Acceptable |
| `small` | 466 MB | Normal | Good |
| `medium` | 1.5 GB | Slow | Very good |
| `large` | 2.9 GB | Very slow | Excellent |
| `turbo` | ~800 MB | Fast | Very good (default) |

## Output formats

```bash
# Plain text (.txt)
whisper audio.mp3 --output_format txt

# SRT subtitles
whisper audio.mp3 --output_format srt

# VTT subtitles
whisper audio.mp3 --output_format vtt

# Full JSON with timestamps
whisper audio.mp3 --output_format json

# All formats at once
whisper audio.mp3 --output_format all
```

## Notes

- Models are automatically downloaded to `~/.cache/whisper` on first use.
- Default model is `turbo`; use `--model medium` for better balance.
- For long audio (> 30 min), use `--model large` for better accuracy.
- Supported formats: mp3, mp4, m4a, wav, ogg, flac, webm, and more (via ffmpeg).
