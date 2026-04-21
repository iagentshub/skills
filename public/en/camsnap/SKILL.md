---
id: CAMSNAP
name: RTSP/ONVIF Cameras (camsnap)
description: Capture snapshots and clips from RTSP/ONVIF IP cameras with the camsnap CLI. Allows adding cameras, taking snapshots, recording clips, and detecting motion.
icon: 📸
category: media
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# RTSP/ONVIF Cameras (camsnap)

Use `camsnap` to capture snapshots, clips, or motion events from configured IP cameras.

## Requirements

- `camsnap` installed
- `ffmpeg` on PATH
- IP cameras with RTSP or ONVIF support on the local network

## Installation

```bash
brew install steipete/tap/camsnap
```

## Configuration

Config file: `~/.config/camsnap/config.yaml`

### Add a camera

```bash
camsnap add --name kitchen --host 192.168.1.10 --user user --pass pass
camsnap add --name entrance --host 192.168.1.11 --user admin --pass admin123
```

## Common commands

```bash
# Discover cameras on the local network
camsnap discover --info

# Snapshot
camsnap snap kitchen --out /tmp/kitchen.jpg

# Record clip
camsnap clip kitchen --dur 5s --out /tmp/clip.mp4

# Motion detection
camsnap watch kitchen --threshold 0.2 --action 'echo "Motion detected"'

# Connection diagnostics
camsnap doctor --probe
```

## Notes

- Requires `ffmpeg` on PATH.
- Run a short test capture before longer recordings.
