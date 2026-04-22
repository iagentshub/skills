---
id: BLOGWATCHER
name: Blog & RSS Feed Monitor
description: Monitor blogs and RSS/Atom feeds for new posts using the blogwatcher CLI. Allows adding blogs, scanning for updates, and marking articles as read.
icon: 📰
category: data
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Blog & RSS Feed Monitor (blogwatcher)

Use `blogwatcher` to track blogs and RSS/Atom feeds and detect new posts.

## Installation

```bash
go install github.com/Hyaxia/blogwatcher/cmd/blogwatcher@latest
```

## Common commands

```bash
# Add a blog/feed
blogwatcher add "Blog Name" https://example.com
blogwatcher add "Hacker News" https://news.ycombinator.com/rss

# List monitored blogs
blogwatcher blogs

# Scan for updates
blogwatcher scan

# View articles (includes new ones)
blogwatcher articles

# Mark article as read (by ID)
blogwatcher read 1

# Mark all as read
blogwatcher read-all

# Remove a blog
blogwatcher remove "Blog Name"
```

## Example workflow

```bash
# 1. Add sources of interest
blogwatcher add "Python Weekly" https://pythonweekly.com/rss

# 2. Scan at the start of the day
blogwatcher scan

# 3. Check new posts
blogwatcher articles

# 4. Mark as read
blogwatcher read-all
```
