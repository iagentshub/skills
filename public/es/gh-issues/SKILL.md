---
id: GITHUB_ISSUES
name: GitHub Issues
description: "Obtiene issues de GitHub, lanza sub-agentes para implementar fixes y abrir PRs, y monitorea los comentarios de revisión. Requiere GH_TOKEN. Uso: /github-issues [owner/repo] [--label bug] [--limit 5] [--milestone v1.0] [--assignee @me] [--fork user/repo] [--watch] [--dry-run]"
icon: 🐛
category: dev
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# GitHub Issues — Resolución automática con sub-agentes

Eres un orquestador. Sigue estas 6 fases en orden. No omitas ninguna.

**IMPORTANTE** — Esta skill usa `curl` + la API REST de GitHub exclusivamente. El token se lee de la variable de entorno `GH_TOKEN`. Pásalo como Bearer token en todas las llamadas:

```bash
curl -s -H "Authorization: Bearer $GH_TOKEN" -H "Accept: application/vnd.github+json" ...
```

---

## Fase 1 — Parsear argumentos

Parsea los argumentos proporcionados tras `/github-issues`.

**Posicional:**
- `owner/repo` — opcional. Si se omite, detecta desde el remote de git actual:
  ```bash
  git remote get-url origin
  ```
  Extrae `owner/repo` de la URL (soporta HTTPS y SSH).

**Flags opcionales:**

| Flag | Por defecto | Descripción |
|------|-------------|-------------|
| `--label` | _(ninguno)_ | Filtrar por etiqueta (ej. `bug`, `enhancement`) |
| `--limit` | 10 | Máximo de issues a obtener |
| `--milestone` | _(ninguno)_ | Filtrar por milestone |
| `--assignee` | _(ninguno)_ | Filtrar por asignado (`@me` para uno mismo) |
| `--state` | open | Estado del issue: `open`, `closed`, `all` |
| `--fork` | _(ninguno)_ | Tu fork (`user/repo`) donde empujar ramas y abrir PRs |
| `--watch` | false | Seguir sondeando nuevos issues tras cada lote |
| `--interval` | 5 | Minutos entre sondeos (solo con `--watch`) |
| `--dry-run` | false | Solo mostrar — sin lanzar sub-agentes |
| `--yes` | false | Confirmar automáticamente todos los issues |
| `--reviews-only` | false | Solo revisar PRs abiertos con comentarios de revisión |

Valores derivados:
- `SOURCE_REPO` = el `owner/repo` posicional (donde viven los issues)
- `PUSH_REPO` = valor de `--fork` si se proporcionó, si no igual a `SOURCE_REPO`
- `FORK_MODE` = `true` si se proporcionó `--fork`

---

## Fase 2 — Obtener issues

Asegúrate de que `GH_TOKEN` esté disponible:

```bash
echo $GH_TOKEN
```

Si está vacío, detente e indica al usuario que exporte `GH_TOKEN` como variable de entorno.

Construye y ejecuta la llamada a la API de GitHub:

```bash
curl -s -H "Authorization: Bearer $GH_TOKEN" -H "Accept: application/vnd.github+json" \
  "https://api.github.com/repos/{SOURCE_REPO}/issues?per_page={limit}&state={state}&{query_params}"
```

Donde `{query_params}` se construye desde `--label`, `--milestone`, `--assignee`.

**IMPORTANTE:** La API de issues también devuelve pull requests. Fíltralos — excluye cualquier elemento donde exista la clave `pull_request` en el objeto respuesta.

Gestión de errores:
- HTTP 401/403 → "Autenticación de GitHub fallida. Verifica tu GH_TOKEN."
- Array vacío → "No se encontraron issues con los filtros indicados."

Para cada issue extrae: `number`, `title`, `body`, `labels`, `assignees`, `html_url`.

---

## Fase 3 — Presentar y confirmar

Muestra una tabla markdown con los issues obtenidos:

| # | Título | Etiquetas |
|---|--------|-----------|
| 42 | Corregir puntero nulo en el parser | bug, crítico |
| 37 | Añadir lógica de reintento para las llamadas API | enhancement |

Si `--fork` está activo, muestra también:
> "Modo fork: las ramas se subirán a `{PUSH_REPO}`, los PRs apuntarán a `{SOURCE_REPO}`"

Si `--dry-run` está activo: muestra la tabla y detente.

Si `--yes` está activo: procesa todos automáticamente.

Si no, pregunta al usuario qué issues procesar:
- `"todos"` — procesar todos
- Números separados por comas (ej. `42, 37`)
- `"cancelar"` — abortar

---

## Fase 4 — Comprobaciones previas

1. **Árbol de trabajo sucio:**
   ```bash
   git status --porcelain
   ```
   Si hay cambios no confirmados, avisa al usuario y espera confirmación.

2. **Registrar rama base:**
   ```bash
   git rev-parse --abbrev-ref HEAD
   ```
   Guarda como `BASE_BRANCH`.

3. **Verificar acceso de escritura:**
   ```bash
   curl -s -H "Authorization: Bearer $GH_TOKEN" \
     -H "Accept: application/vnd.github+json" \
     "https://api.github.com/repos/{PUSH_REPO}"
   ```
   Verifica que el campo `permissions.push` es `true`.

---

## Fase 5 — Lanzar sub-agentes

Para cada issue confirmado, lanza un sub-agente en segundo plano con:

```
Implementa una solución para el issue #<número>: <título>

Repo: <owner/repo>
Issue body: <body>
Rama base: <BASE_BRANCH>

Pasos:
1. git checkout -b fix/issue-<número>
2. Implementa la solución
3. git add . && git commit -m "fix: <título> (#<número>)"
4. git push origin fix/issue-<número>
5. Abre un PR con: título "fix: <título>", cuerpo "Closes #<número>"
```

Monitoriza los sub-agentes y reporta el progreso a medida que terminen.

---

## Fase 6 — Revisar comentarios de PR

Para cada PR abierto relacionado con los issues procesados:

1. Obtén los comentarios de revisión:
   ```bash
   curl -s -H "Authorization: Bearer $GH_TOKEN" \
     -H "Accept: application/vnd.github+json" \
     "https://api.github.com/repos/{SOURCE_REPO}/pulls/{pr_number}/reviews"
   ```

2. Si hay comentarios sin resolver, lanza un sub-agente para abordarlos.

3. Tras los cambios, actualiza el PR y responde a los comentarios marcándolos como resueltos.
