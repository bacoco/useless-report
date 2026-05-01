---
name: ingest-from-git
description: >
  Reads git history from the current repository to extract structured engineering activity for a
  given period — commits, files touched, contributors, branches, and ticket references found in
  commit messages. Use this skill when the user wants to generate a manager report from real git
  data, asks "what did we ship this week", "summarize the recent activity", "what's been
  happening in this repo", or before any useless-report generate-* skill when working inside a
  git repository. Also triggers when the user mentions weekly status, sprint summary, or wants
  to feed actual repo activity into the useless-report pipeline rather than typing a summary
  manually. Outputs a structured fact base ready to feed into any generate-* skill.
---

# ingest-from-git

## Goal

Extract a factual summary of git activity from the current repository for a given time period. The output is a structured fact base — never a report. The downstream `generate-*` skill is responsible for narrative shape; this skill only collects the truth.

> Facts in. Narrative later.

## How to use

You are operating in a git repository (the user's current working directory). Use the **Bash tool** to gather data — do not invent commits.

### Step 1 — Confirm the period

Default: **last 7 days**. If the user specifies otherwise (e.g., "last 2 weeks", "since last Monday", "this sprint"), use that. Convert relative dates to git's `--since=` format.

### Step 2 — Run git commands

Use these commands. Run them in parallel where possible:

```bash
# Verify we're in a git repo
git rev-parse --is-inside-work-tree

# Repo info
git config --get remote.origin.url
git rev-parse --abbrev-ref HEAD

# All commits in the period (no merges by default)
git log --since="<period>" --no-merges --pretty=format:"%h|%an|%ae|%ad|%s" --date=short

# Per-commit file stats
git log --since="<period>" --no-merges --shortstat --pretty=format:"---%n%h|%s"

# Files touched (deduplicated)
git log --since="<period>" --no-merges --name-only --pretty=format:""

# Total stats summary
git log --since="<period>" --no-merges --shortstat | tail -1

# Recent branches
git for-each-ref --sort=-committerdate refs/heads/ \
  --format='%(refname:short)|%(committerdate:short)|%(authorname)' | head -20

# Merge commits separately (often reflect PRs)
git log --since="<period>" --merges --pretty=format:"%h|%an|%ad|%s" --date=short
```

### Step 3 — Extract structured facts

From the raw output, extract:

- **Commits**: list of `(hash, author, date, subject)`
- **Ticket references**: regex `[A-Z]{2,}-\d+` in commit messages (e.g., `AUTH-184`, `BILL-77`)
- **Conventional commit types**: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, etc. — count each
- **Files most touched**: top 10 files by commit frequency
- **Contributors**: distinct authors and their commit counts
- **Period totals**: total commits, files changed, lines added, lines removed
- **Active branches**: branches with commits in the period
- **Merges / likely PRs**: merge commits with their messages

### Step 4 — Output

Produce a single markdown block that any `generate-*` skill can consume directly:

```md
## Git Activity Summary — [start] to [end]

**Repo:** [name from remote URL or directory name]
**Branch:** [current branch]
**Total commits:** N (excluding merges)
**Contributors:** N — [name1 (X), name2 (Y), ...]
**Lines changed:** +X / -Y across N files

### Commit types
- feat: N
- fix: N
- refactor: N
- ...

### Tickets referenced
AUTH-184, BILL-77, ...

### Notable commits
- [hash] [date] [author] — [subject]
- ... (10–15 most significant; pick by message length and ticket presence)

### Files most touched
- src/auth/session.ts — 5 commits
- ...

### Merges / PRs (if any)
- [hash] [date] — [merge message]
- ...

### Branches with recent activity
- main: last commit [date] by [author]
- ...
```

## Rules

- **Never fabricate.** Only report what `git log` shows. If you can't determine something, say so.
- **No git repo?** If `git rev-parse --is-inside-work-tree` fails, return: *"Not inside a git repository. useless-report:ingest-from-git requires a git repo. Run from inside your project, or provide a manual summary instead."*
- **Empty period?** If no commits in the range, say: *"No commits found between [start] and [end]."*
- **Filter merge commits** by default (use `--no-merges`) — they bloat the activity but report them in their own section.
- **Don't truncate commit subjects** — downstream skills need the full context.
- **Respect privacy**: report only what's already in git history. Don't try to read author emails into a public output.

## Pipeline position

```
ingest-from-git
    ↓
[classify-manager-style]   (optional, if profile not yet known)
    ↓
generate-X-report          (X = the archetype)
    ↓
[format-as-html | format-as-slides]   (optional rendering)
```

The output of this skill is the fact base. Pass it to a `generate-*` skill verbatim.

## Tip

Combine with `useless-report:ingest-from-github` to enrich with PR titles, review state, and linked issues. Combine outputs by appending sections.
