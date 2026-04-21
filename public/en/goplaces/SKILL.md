---
id: GOOGLE_PLACES
name: Google Places API
description: Query the Google Places API (New) via the goplaces CLI to search places, get details, resolve names, and read reviews. Requires GOOGLE_PLACES_API_KEY.
icon: 📍
category: data
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Google Places API (goplaces)

Modern CLI for the Google Places API. Human-friendly output by default, `--json` for scripts.

## Requirements

- `goplaces` installed
- `GOOGLE_PLACES_API_KEY` environment variable set

## Installation

```bash
brew install steipete/tap/goplaces
```

## Configuration

```bash
export GOOGLE_PLACES_API_KEY="your-api-key-here"
# Optional: proxy/testing
export GOOGLE_PLACES_BASE_URL="https://your-proxy/maps"
```

## Commands

### Search places

```bash
# Basic search
goplaces search "coffee shop New York"

# With filters
goplaces search "Italian restaurant" --open-now --min-rating 4 --limit 5

# With location and radius
goplaces search "pizza" --lat 40.712 --lng -74.006 --radius-m 2000

# By type
goplaces search "hospital" --type hospital

# Pagination
goplaces search "bar" --page-token "NEXT_PAGE_TOKEN"
```

### Resolve name/address to place_id

```bash
goplaces resolve "Times Square, New York" --limit 5
goplaces resolve "Eiffel Tower" --limit 3
```

### Place details

```bash
goplaces details <place_id>
goplaces details <place_id> --reviews
```

### JSON output for scripts

```bash
goplaces search "sushi" --json
goplaces details <place_id> --json
```

## Usage notes

- `--no-color` or `NO_COLOR=1` disables ANSI color.
- Price levels range from 0 (free) to 4 (very expensive).
- The `--type` filter only accepts one type (Google API limitation).
