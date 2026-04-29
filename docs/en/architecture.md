<div align="center">
  <a href="index.md">← Index</a> &nbsp;·&nbsp;
  <a href="../es/architecture.md">🇪🇸 Ver en Español</a>
</div>

<br>

# Skills Architecture

---

## What a skill is

A skill is a set of instructions that extends an agent's capabilities. When an agent has a skill enabled, those instructions are automatically incorporated into its behavior. The agent knows what it can do and how to do it, without any additional configuration.

---

## Types of skills

**Public skills** — skills from the official catalog, available to all agents. Automatically synced from this repository on every platform startup.

**Private skills** — skills specific to an installation, not included in the public catalog. Stored locally and not distributed.

---

## Languages

The catalog is available in multiple languages. Each skill exists in all available language versions with the same identifier across all of them. The platform serves the catalog in the language configured by the administrator.

---

## Categories

Skills are organized by category according to their main function:

| Category | What it includes |
|---|---|
| **ai** | Integration with other AI models and generation tools |
| **data** | External data queries (weather, places, APIs) |
| **dev** | Development tools (GitHub, version control, terminal) |
| **media** | Audio, video, and image capture and processing |
| **messaging** | Communication (email, messaging, social networks) |
| **notes** | Note and document management |
| **productivity** | Productivity and task management |
| **security** | System auditing and security |

---

## How they are distributed

This repository is the source of truth for the public catalog. The platform automatically downloads skills on every startup, so any update to the repository is reflected on the next launch without manual intervention.

---

## How to contribute

Each skill is a text file with a metadata header (name, description, icon, category) followed by the instructions. To add a skill to the catalog, it must exist in all available languages with the same identifier. The continuous integration system verifies this consistency automatically.
