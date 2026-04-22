---
id: GITHUB_ISSUES
name: GitHub Issues
description: "Fetch GitHub issues, spawn sub-agents to implement fixes and open PRs, and monitor review comments. Requires GH_TOKEN. Usage: /github-issues [owner/repo] [--label bug] [--limit 5] [--milestone v1.0] [--assignee @me] [--fork user/repo] [--watch] [--dry-run]"
icon: 🐛
category: dev
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# GitHub Issues — Automatic resolution with sub-agents

You are an orchestrator. Follow these 6 phases in order. Do not skip any.

**IMPORTANT** — This skill uses `curl` + the GitHub REST API exclusively. The token is read from the `GH_TOKEN` environment variable. Pass it as a Bearer token in all calls:

```bash
curl -s -H "Authorization: Bearer $GH_TOKEN" -H "Accept: application/vnd.github+json" ...
```

---

## Phase 1 — Parse arguments

Parse the arguments provided after `/github-issues`.

**Positional:**
- `owner/repo` — optional. If omitted, detect from current git remote:
  ```bash
  git remote get-url origin
  ```
  Extract `owner/repo` from the URL (supports HTTPS and SSH).

**Optional flags:**

| Flag | Default | Description |
|------|---------|-------------|
| `--label` | _(none)_ | Filter by label (e.g. `bug`, `enhancement`) |
| `--limit` | 10 | Max issues to fetch |
| `--milestone` | _(none)_ | Filter by milestone |
| `--assignee` | _(none)_ | Filter by assignee (`@me` for self) |
| `--state` | open | Issue state: `open`, `closed`, `all` |
| `--fork` | _(none)_ | Your fork (`user/repo`) to push branches and open PRs |
| `--watch` | false | Keep polling for new issues after each batch |
| `--interval` | 5 | Minutes between polls (only with `--watch`) |
| `--dry-run` | false | Show only — do not spawn sub-agents |
| `--yes` | false | Auto-confirm all issues |
| `--reviews-only` | false | Only review open PRs with review comments |

Derived values:
- `SOURCE_REPO` = the positional `owner/repo` (where issues live)
- `PUSH_REPO` = value of `--fork` if provided, otherwise same as `SOURCE_REPO`
- `FORK_MODE` = `true` if `--fork` was provided

---

## Phase 2 — Fetch issues

Ensure `GH_TOKEN` is available:

```bash
echo $GH_TOKEN
```

If empty, stop and tell the user to export `GH_TOKEN` as an environment variable.

Build and execute the GitHub API call:

```bash
curl -s -H "Authorization: Bearer $GH_TOKEN" -H "Accept: application/vnd.github+json" \
  "https://api.github.com/repos/{SOURCE_REPO}/issues?per_page={limit}&state={state}&{query_params}"
```

Where `{query_params}` is built from `--label`, `--milestone`, `--assignee`.

**IMPORTANT:** The issues API also returns pull requests. Filter them out — exclude any item where `pull_request` key exists in the response object.

Error handling:
- HTTP 401/403 → "GitHub authentication failed. Check your GH_TOKEN."
- Empty array → "No issues found with the specified filters."

For each issue extract: `number`, `title`, `body`, `labels`, `assignees`, `html_url`.

---

## Phase 3 — Present and confirm

Show a markdown table with the fetched issues:

| # | Title | Labels |
|---|-------|--------|
| 42 | Fix null pointer in parser | bug, critical |
| 37 | Add retry logic for API calls | enhancement |

If `--fork` is active, also show:
> "Fork mode: branches will be pushed to `{PUSH_REPO}`, PRs will target `{SOURCE_REPO}`"

If `--dry-run` is active: show the table and stop.

If `--yes` is active: process all automatically.

Otherwise ask the user which issues to process:
- `"all"` — process all
- Comma-separated numbers (e.g. `42, 37`)
- `"cancel"` — abort

---

## Phase 4 — Pre-flight checks

1. **Dirty working tree:**
   ```bash
   git status --porcelain
   ```
   If there are uncommitted changes, warn the user and wait for confirmation.

2. **Record base branch:**
   ```bash
   git rev-parse --abbrev-ref HEAD
   ```

---

## Phase 5 — Spawn sub-agents

For each selected issue, spawn a sub-agent with the task:

> "Fix issue #{number}: {title}\n\n{body}\n\nCreate a branch `fix/issue-{number}`, implement the fix, commit, push to {PUSH_REPO}, and open a PR targeting {SOURCE_REPO}."

Monitor progress with `process action:log`.

---

## Phase 6 — Monitor and report

After all sub-agents finish:
- Show a summary table with each issue, branch, and PR link.
- If `--watch` is active, wait `--interval` minutes and go back to Phase 2.
