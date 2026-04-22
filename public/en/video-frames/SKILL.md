---
id: VIDEO_FRAMES
name: Video Frame Extraction
description: Extract frames or short clips from videos using ffmpeg. Useful for capturing screenshots at specific timestamps or creating thumbnails for visual inspection.
icon: 🎬
category: media
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Video Frame Extraction (ffmpeg)

Use `ffmpeg` to extract a specific frame or create thumbnails from a video for visual inspection.

## Requirements

- `ffmpeg` installed on PATH

## Installation

```bash
brew install ffmpeg
```

## Commands

### Extract a frame

```bash
# First frame
ffmpeg -i video.mp4 -frames:v 1 frame.jpg

# At a specific timestamp (HH:MM:SS)
ffmpeg -i video.mp4 -ss 00:00:10 -frames:v 1 frame_10s.jpg

# High quality
ffmpeg -i video.mp4 -ss 00:01:30 -frames:v 1 -q:v 2 frame.jpg

# As PNG (lossless)
ffmpeg -i video.mp4 -ss 00:00:05 -frames:v 1 frame.png
```

### Create thumbnails at intervals

```bash
# One thumbnail every 30 seconds
ffmpeg -i video.mp4 -vf fps=1/30 /tmp/thumb%03d.jpg

# One thumbnail every 10 seconds
ffmpeg -i video.mp4 -vf fps=1/10 /tmp/thumb%03d.jpg

# One thumbnail per second
ffmpeg -i video.mp4 -vf fps=1 /tmp/thumb%03d.jpg
```

### Extract short clip

```bash
# 5-second clip starting at second 30
ffmpeg -i video.mp4 -ss 00:00:30 -t 5 -c copy clip.mp4

# 10-second clip starting at minute 2
ffmpeg -i video.mp4 -ss 00:02:00 -t 10 -c copy clip.mp4
```

### Get video info

```bash
ffmpeg -i video.mp4 2>&1 | grep -E "Duration|Stream"
```

## Notes

- Use `-ss` before `-i` for fast seeking (less precise) or after `-i` for frame-accurate seeking (slower).
- `.jpg` for quick sharing; `.png` for UI screenshots with text (no artifacts).
- For very long videos, use fast seeking (`-ss` before `-i`) for better performance.
- `-q:v 2` gives maximum JPEG quality (range 1-31, lower = better).
