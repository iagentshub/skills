---
id: APPLE_REMINDERS
name: Apple Reminders CLI
description: Manage Apple Reminders from the terminal with remindctl. List, create, complete, and delete reminders with date and list support. Syncs with iOS/iPadOS.
icon: ⏰
category: productivity
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Apple Reminders CLI (remindctl)

> ⚠️ **macOS only.** Reminders sync via iCloud (iPhone/iPad).

Use `remindctl` to manage Apple Reminders directly from the terminal.

## When to use

✅ **USE this skill when:**

- User explicitly mentions "reminder" or "Reminders app"
- Creating personal tasks with due dates that sync to iOS
- Managing Apple Reminders lists
- User wants tasks to appear in their iPhone/iPad

## When NOT to use

❌ **DON'T use this skill when:**

- Scheduling agent alerts → use cron or the task scheduler
- Calendar events or appointments → use Apple Calendar
- Project/work task management → use Notion, GitHub Issues, or other tools
- User says "remind me" but means an agent alert → clarify first

## Installation

```bash
brew install steipete/tap/remindctl
```

Grant access to Reminders when prompted.

## View reminders

```bash
remindctl               # Today's reminders
remindctl today         # Today
remindctl tomorrow      # Tomorrow
remindctl week          # This week
remindctl overdue       # Past due
remindctl all           # All reminders
remindctl 2026-05-01    # Specific date
```

## Manage lists

```bash
remindctl list                           # View all lists
remindctl list Work                      # View specific list
remindctl list "Shopping" --create       # Create list
remindctl list "Shopping" --delete       # Delete list
```

## Create reminders

```bash
remindctl add "Buy bread"
remindctl add --title "Call mom" --list Personal --due tomorrow
remindctl add --title "Team meeting" --due "2026-05-10 09:00"
remindctl add --title "Medical checkup" --list Health --due "2026-06-01"
```

## Complete and delete

```bash
remindctl complete 1 2 3       # Complete by ID
remindctl delete 4A83 --force  # Delete by ID
```

## Output formats

```bash
remindctl today --json         # JSON for scripting
remindctl today --plain        # TSV format
remindctl today --quiet        # Counts only
```
