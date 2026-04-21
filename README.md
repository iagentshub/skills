# iAgentsHub — Skills

Public skills repository for the [iAgentsHub](https://github.com/iagentshub) agent manager. Skills are reusable instruction sets that extend agent capabilities.

[![Check Skills Parity](https://github.com/iagentshub/skills/actions/workflows/check-skills-parity.yml/badge.svg)](https://github.com/iagentshub/skills/actions/workflows/check-skills-parity.yml)

---

## Structure

```
public/
  {lang}/
    {skill-id}/
      SKILL.md
docs/
  en.md   ← English skills catalog
  es.md   ← Catálogo de skills en español
```

Each `SKILL.md` contains a YAML frontmatter block (`name`, `description`, `icon`, `category`) followed by the skill instructions.

The same slug must exist in every language folder — enforced automatically by the CI workflow.

---

## Usage

### Clone directly

```bash
git clone https://github.com/iagentshub/skills.git
```

### Use as a git submodule

```bash
# Add to your project
git submodule add https://github.com/iagentshub/skills.git skills

# When cloning a project that already uses it
git clone --recurse-submodules <your-repo>

# Update to latest
git submodule update --remote skills
```

Skills are then accessible at:

```
skills/public/{lang}/{skill-id}/SKILL.md
```

---

## Stats

| | Count |
|---|---|
| Total skills | 33 |
| Total categories | 8 |

Categories: `ai` · `data` · `dev` · `media` · `messaging` · `notes` · `productivity` · `security`

---

## Languages

| Language | Catalog | Folder |
|----------|---------|--------|
| English  | [docs/en.md](docs/en.md) | `public/en/` |
| Español  | [docs/es.md](docs/es.md) | `public/es/` |

---

See each language catalog for contributing guidelines and full skill descriptions.

---

## License

MIT © [iAgentsHub](https://github.com/iagentshub)