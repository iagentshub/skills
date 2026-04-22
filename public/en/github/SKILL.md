---
id: GITHUB
name: GitHub
description: "GitHub operations with the gh CLI: issues, PRs, CI runs, code reviews, and API queries. Use when: checking PR or CI status, creating/commenting issues, listing PRs, viewing run logs. NOT for: local git operations (commit, push, pull), cloning repos, or when gh is not authenticated."
icon: 🐙
category: dev
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# GitHub

Use the `gh` CLI to interact with repositories, issues, PRs, and GitHub CI.

## When to use

✅ **USE this skill when:**

- Checking PR status, reviews, or merge readiness
- Viewing CI/workflow run status and logs
- Creating, closing, or commenting on issues
- Creating or merging pull requests
- Querying the GitHub API for repository data
- Listing repos, releases, or collaborators

❌ **DON'T use this skill when:**

- Local git operations (commit, push, pull, branch) → use `git` directly
- Non-GitHub repos (GitLab, Bitbucket, self-hosted)
- Cloning repos → use `git clone`
- Reviewing code changes → use the coding agent
- Large multi-file diffs → read the files directly

## Setup

```bash
# Authenticate (once)
gh auth login

# Verify
gh auth status
```

## Common commands

### Pull Requests

```bash
# List PRs
gh pr list --repo owner/repo

# Check CI for a PR
gh pr checks 55 --repo owner/repo

# View PR details
gh pr view 55 --repo owner/repo

# Create PR
gh pr create --title "feat: new feature" --body "Description"

# Merge PR
gh pr merge 55 --squash --repo owner/repo
```

### Issues

```bash
# List issues
gh issue list --repo owner/repo --state open

# Create issue
gh issue create --title "Bug: something broken" --body "Details..."

# Close issue
gh issue close 42 --repo owner/repo
```

### CI/Workflow runs

```bash
# List recent runs
gh run list --repo owner/repo --limit 10

# View specific run
gh run view <run-id> --repo owner/repo

# View failed step logs only
gh run view <run-id> --repo owner/repo --log-failed

# Re-run failed jobs
gh run rerun <run-id> --failed --repo owner/repo
```

### API queries

```bash
# Get a PR with specific fields
gh api repos/owner/repo/pulls/55 --jq '.title, .state, .user.login'

# List all labels
gh api repos/owner/repo/labels --jq '.[].name'
```
