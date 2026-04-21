---
id: MACOS_UI_AUTOMATION
name: macOS UI Automation (Peekaboo)
description: Capture screenshots and automate the macOS user interface with the Peekaboo CLI. Allows clicking, typing, navigating menus, and controlling apps. macOS only.
icon: 👀
category: dev
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: macOS UI Automation (Peekaboo)

> ⚠️ **macOS only.** Requires Screen Recording and Accessibility permissions.

Peekaboo is a full macOS UI automation CLI: capture/inspect screens, interact with UI elements, control input, and manage apps, windows, and menus.

## Requirements

- macOS
- `peekaboo` installed (`brew install steipete/tap/peekaboo`)
- **Screen Recording** and **Accessibility** permissions granted to the app running the CLI

## Installation

```bash
brew install steipete/tap/peekaboo
```

## Check permissions

```bash
peekaboo permissions
```

## Main commands

### Capture and inspect

```bash
# View available apps
peekaboo list apps --json

# Capture screen and annotate UI elements
peekaboo see --annotate --path /tmp/screen.png

# Capture specific window
peekaboo image --mode window --app Safari --path /tmp/safari.png
```

### Interaction

```bash
# Click an element (use ID from `see` snapshot)
peekaboo click --on B1

# Type text
peekaboo type "Hello world" --return

# Paste text from clipboard
peekaboo paste "Text to paste"

# Keyboard shortcut
peekaboo hotkey cmd,shift,t

# Scroll
peekaboo scroll --direction down --amount 3
```

### App management

```bash
# Launch app
peekaboo app --action launch --app "Safari"

# Switch to app
peekaboo app --action switch --app "Terminal"

# List open apps
peekaboo list apps
```

### Window management

```bash
# List windows
peekaboo list windows

# Focus window
peekaboo window --action focus --window-title "GitHub"

# Maximize window
peekaboo window --action maximize --app "Finder"
```

### Menus

```bash
# Click a menu item
peekaboo menu --app Safari --path "File > New Window"
```

## Notes

- Always check permissions first with `peekaboo permissions`.
- Use `peekaboo see --annotate` to identify element IDs before clicking.
- macOS only; requires Screen Recording and Accessibility permissions granted.
