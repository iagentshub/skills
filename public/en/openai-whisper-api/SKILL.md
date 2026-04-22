---
id: API_TRANSCRIPTION
name: Audio Transcription via API (Whisper)
description: Transcribe audio via the OpenAI Audio Transcriptions API (Whisper). Requires OPENAI_API_KEY. Ideal when you don't want to install local dependencies.
icon: 🌐
category: media
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Audio Transcription via API (Whisper)

Transcribe audio files using OpenAI's `/v1/audio/transcriptions` endpoint (model `whisper-1`).

## Requirements

- `curl` on PATH
- `OPENAI_API_KEY` environment variable set

## Basic usage

```bash
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@/path/to/audio.m4a \
  -F model=whisper-1
```

## With additional parameters

```bash
# Specify language (improves accuracy)
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@audio.mp3 \
  -F model=whisper-1 \
  -F language=en

# With prompt to guide the transcription
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@audio.mp3 \
  -F model=whisper-1 \
  -F prompt="Sales meeting with John Smith and Jane Doe"

# JSON response with timestamps
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@audio.mp3 \
  -F model=whisper-1 \
  -F response_format=verbose_json
```

## Save result to file

```bash
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@meeting.m4a \
  -F model=whisper-1 \
  -o transcription.txt
```

## Response formats

- `json` (default): `{"text": "transcription here"}`
- `text`: plain text
- `srt`: SRT subtitles
- `vtt`: VTT subtitles
- `verbose_json`: JSON with segments and timestamps

## Supported audio formats

mp3, mp4, mpeg, mpga, m4a, wav, webm (max 25 MB per file).

## Notes

- If the file exceeds 25 MB, split it with `ffmpeg` before uploading.
- `OPENAI_BASE_URL` can point to a proxy or local gateway compatible with the OpenAI API.
- Never share or print the value of `OPENAI_API_KEY` in logs or agent output.
