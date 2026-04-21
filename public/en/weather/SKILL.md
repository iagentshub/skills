---
id: WEATHER
name: Weather & Forecast
description: Get current weather and forecasts via wttr.in. Use when the user asks about weather, temperature, or forecasts for any location. No API key required.
icon: ☔
category: data
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: Weather & Forecast

Get current weather conditions and forecasts using `wttr.in` via `curl`. No API key needed.

## When to use

✅ **USE this skill when:**

- "What's the weather like?"
- "Will it rain today/tomorrow?"
- "Temperature in [city]"
- "Week forecast"
- Travel planning

## When NOT to use

❌ **DON'T use this skill when:**

- Historical weather data → use weather archives/APIs
- Climate analysis or trends → use specialized data sources
- Severe weather alerts → check official sources (NWS, etc.)
- Aviation/nautical weather → use specialized services (METAR, etc.)

## Requirements

- `curl` on PATH

## Commands

### Current weather

```bash
# One-line summary
curl "wttr.in/London?format=3"

# Detailed current conditions
curl "wttr.in/London?0"

# Specific city
curl "wttr.in/New+York?format=3"
```

### Forecasts

```bash
# 3-day forecast
curl "wttr.in/London"

# Extended format
curl "wttr.in/London?format=v2"

# Specific day (0=today, 1=tomorrow, 2=day after)
curl "wttr.in/London?1"
```

### Format options

```bash
# One custom line
curl "wttr.in/London?format=%l:+%c+%t+%w"

# JSON output
curl "wttr.in/London?format=j1"

# PNG image
curl "wttr.in/London.png"
```

### Format codes

- `%c` — Condition emoji
- `%t` — Temperature
- `%f` — Feels like
- `%w` — Wind
- `%h` — Humidity
- `%p` — Precipitation
- `%l` — Location

## Quick examples

**"What's the weather like?"**

```bash
curl -s "wttr.in/London?format=%l:+%c+%t+(feels+%f),+wind+%w,+humidity+%h"
```

**"Will it rain?"**

```bash
curl -s "wttr.in/London?format=%l:+%c+%p"
```
