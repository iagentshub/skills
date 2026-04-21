---
id: GITHUB
name: GitHub
description: "Operaciones en GitHub con el CLI gh: issues, PRs, ejecuciones de CI, revisiones de código y consultas a la API. Usa cuando necesites: comprobar estado de PRs o CI, crear/comentar issues, listar PRs, ver logs de ejecuciones. NO usar para: operaciones locales de git (commit, push, pull), clonar repositorios, o cuando gh no está autenticado."
icon: 🐙
category: dev
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# GitHub

Usa el CLI `gh` para interactuar con repositorios, issues, PRs y CI de GitHub.

## Cuándo usar

✅ **USA esta skill cuando:**

- Comprobar estado de PRs, revisiones o si está listo para merge
- Ver estado de ejecuciones CI/workflow y sus logs
- Crear, cerrar o comentar issues
- Crear o mergear pull requests
- Consultar la API de GitHub sobre datos de repositorios
- Listar repos, releases o colaboradores

❌ **NO uses esta skill cuando:**

- Operaciones locales de git (commit, push, pull, branch) → usa `git` directamente
- Repos que no son de GitHub (GitLab, Bitbucket, auto-hospedado)
- Clonar repositorios → usa `git clone`
- Revisar cambios de código → usa el agente de código
- Diffs grandes de múltiples ficheros → lee los ficheros directamente

## Configuración

```bash
# Autenticar (una sola vez)
gh auth login

# Verificar
gh auth status
```

## Comandos comunes

### Pull Requests

```bash
# Listar PRs
gh pr list --repo owner/repo

# Comprobar CI del PR
gh pr checks 55 --repo owner/repo

# Ver detalles del PR
gh pr view 55 --repo owner/repo

# Crear PR
gh pr create --title "feat: nueva funcionalidad" --body "Descripción"

# Mergear PR
gh pr merge 55 --squash --repo owner/repo
```

### Issues

```bash
# Listar issues
gh issue list --repo owner/repo --state open

# Crear issue
gh issue create --title "Bug: algo roto" --body "Detalles..."

# Cerrar issue
gh issue close 42 --repo owner/repo
```

### Ejecuciones CI/Workflow

```bash
# Listar ejecuciones recientes
gh run list --repo owner/repo --limit 10

# Ver ejecución concreta
gh run view <run-id> --repo owner/repo

# Ver solo los logs de pasos fallidos
gh run view <run-id> --repo owner/repo --log-failed

# Re-ejecutar los jobs fallidos
gh run rerun <run-id> --failed --repo owner/repo
```

### Consultas a la API

```bash
# Obtener un PR con campos específicos
gh api repos/owner/repo/pulls/55 --jq '.title, .state, .user.login'

# Listar todas las etiquetas
gh api repos/owner/repo/labels --jq '.[].name'

# Estadísticas del repo
gh api repos/owner/repo --jq '{stars: .stargazers_count, forks: .forks_count}'
```

## Salida JSON

La mayoría de comandos soportan `--json` con filtrado `--jq`:

```bash
gh issue list --repo owner/repo --json number,title --jq '.[] | "\(.number): \(.title)"'
gh pr list --json number,title,state,mergeable --jq '.[] | select(.mergeable == "MERGEABLE")'
```

## Plantilla — Resumen de revisión de PR

```bash
PR=55 REPO=owner/repo
echo "## Resumen PR #$PR"
gh pr view $PR --repo $REPO --json title,body,author,additions,deletions,changedFiles \
  --jq '"**\(.title)** por @\(.author.login)\n\n\(.body)\n\n📊 +\(.additions) -\(.deletions) en \(.changedFiles) ficheros"'
gh pr checks $PR --repo $REPO
```
