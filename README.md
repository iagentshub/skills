<div align="center">
  <a href="docs/en.md">🇬🇧 Read in English</a> &nbsp;·&nbsp;
  <a href="docs/es.md">🇪🇸 Ver en Español</a>
</div>

<br>

<h1 align="center">iAgentsHub — Skills</h1>

<p align="center">Public skills repository for <a href="https://github.com/iagentshub">iAgentsHub</a>. Skills are reusable instruction sets that extend agent capabilities across any supported language.</p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-22c55e?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/skills-33-6366f1?style=flat-square" alt="Skills">
  <img src="https://img.shields.io/badge/languages-2-0891b2?style=flat-square" alt="Languages">
</p>

---

## Stats

![ai](https://img.shields.io/badge/ai-7C3AED?style=for-the-badge&logoColor=white)
![data](https://img.shields.io/badge/data-2563EB?style=for-the-badge&logoColor=white)
![dev](https://img.shields.io/badge/dev-16A34A?style=for-the-badge&logoColor=white)
![media](https://img.shields.io/badge/media-EA580C?style=for-the-badge&logoColor=white)
![messaging](https://img.shields.io/badge/messaging-0891B2?style=for-the-badge&logoColor=white)
![notes](https://img.shields.io/badge/notes-CA8A04?style=for-the-badge&logoColor=white)
![productivity](https://img.shields.io/badge/productivity-DC2626?style=for-the-badge&logoColor=white)
![security](https://img.shields.io/badge/security-BE123C?style=for-the-badge&logoColor=white)

> **33 skills** across **8 categories** in **2 languages**

---

## Languages

| | Language | Catalog | Folder |
|---|---|---|---|
| 🇬🇧 | English | [docs/en.md](docs/en.md) | `public/en/` |
| 🇪🇸 | Español | [docs/es.md](docs/es.md) | `public/es/` |

See each language catalog for contributing guidelines and the full skills list.

---

## CI / Quality

Every push and pull request runs automated checks to keep the repository consistent:

| Workflow | Trigger | What it does |
|---|---|---|
| [![Parity](https://github.com/iagentshub/skills/actions/workflows/check-skills-parity.yml/badge.svg)](https://github.com/iagentshub/skills/actions/workflows/check-skills-parity.yml) | Changes in `public/` | Ensures every skill exists in all languages with an identical slug |
| [![Frontmatter](https://github.com/iagentshub/skills/actions/workflows/validate-frontmatter.yml/badge.svg)](https://github.com/iagentshub/skills/actions/workflows/validate-frontmatter.yml) | Changes in `public/` | Verifies each `SKILL.md` has the required `name` and `description` fields |
| [![Markdown](https://github.com/iagentshub/skills/actions/workflows/lint-markdown.yml/badge.svg)](https://github.com/iagentshub/skills/actions/workflows/lint-markdown.yml) | Changes to any `.md` file | Validates Markdown syntax across all documentation |
| [![Stats](https://github.com/iagentshub/skills/actions/workflows/update-stats.yml/badge.svg)](https://github.com/iagentshub/skills/actions/workflows/update-stats.yml) | Merge to `main` | Auto-updates skill count in README and catalogs |

PRs are also labeled automatically (`new-skill`, `translation`, `docs`, `ci`) based on the files changed.

---

## Structure

```
private/
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

### Clone

```bash
git clone https://github.com/iagentshub/skills.git
```

### Git submodule

```bash
# Add to your project
git submodule add https://github.com/iagentshub/skills.git skills

# Clone a project that already uses it
git clone --recurse-submodules <your-repo>

# Update to latest
git submodule update --remote skills
```

Skills are then accessible at:

```
skills/public/{lang}/{skill-id}/SKILL.md
```

---

## License

MIT © [iAgentsHub](https://github.com/iagentshub)