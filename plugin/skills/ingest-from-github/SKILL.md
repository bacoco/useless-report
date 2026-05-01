---
name: ingest-from-github
description: >
  Fetches pull request and issue activity from GitHub for the current repository using the gh CLI.
  Extracts PR titles, status, review state, linked issues, and recent comments for a given period.
  Use this skill when the user wants to enrich a useless-report manager update with PR-level
  activity (not just commits), when they ask "summarize the PRs from this week", "what got merged",
  "what's still open", or before generating a manager report that benefits from PR context. Also
  triggers on "pull requests this sprint", "issue activity", or when working in a GitHub-hosted
  repository and wanting more than just git log data. Requires the gh CLI to be installed and
  authenticated.
---

# ingest-from-github

## Goal

Pull structured PR and issue activity from GitHub for the current repository, complementing what `ingest-from-git` provides from local git history. This adds the human layer: review state, comments, linked issues, and merge timing.

## Prerequisites

The `gh` CLI must be installed and authenticated. Verify first:

```bash
gh auth status
```

If unauthenticated or missing, return: *"gh CLI not available or not authenticated. Run `gh auth login`, or skip GitHub ingestion and rely on git-only data."*

## How to use

### Step 1 — Confirm the period

Default: **last 7 days**. Match whatever was used for `ingest-from-git` if running both.

### Step 2 — Fetch PRs

```bash
# All PRs touched in the period (created, updated, or closed)
gh pr list --state all --search "updated:>=<YYYY-MM-DD>" \
  --json number,title,author,state,createdAt,closedAt,mergedAt,labels,reviews,additions,deletions,baseRefName,url \
  --limit 100

# Recently merged PRs specifically
gh pr list --state merged --search "merged:>=<YYYY-MM-DD>" \
  --json number,title,author,mergedAt,additions,deletions,labels,url \
  --limit 50

# Open PRs requiring attention
gh pr list --state open \
  --json number,title,author,reviewDecision,isDraft,createdAt,url \
  --limit 50
```

### Step 3 — Fetch issues

```bash
# Issues touched in the period
gh issue list --state all --search "updated:>=<YYYY-MM-DD>" \
  --json number,title,state,labels,createdAt,closedAt,url \
  --limit 50
```

### Step 4 — Extract structured facts

- **Merged PRs**: list with title, author, merge date, +/- lines, labels
- **Open PRs**: title, author, review state (APPROVED, REVIEW_REQUIRED, CHANGES_REQUESTED), is-draft
- **Closed without merge**: PRs that were rejected or abandoned
- **Issues closed**: bug fixes, features, chores
- **Issues opened**: new work surfaced in the period
- **Review activity**: PRs that received reviews this period
- **Linked work**: PRs that reference issues (look for `#NNN` in titles or bodies)

### Step 5 — Output

```md
## GitHub Activity Summary — [start] to [end]

**Repo:** [owner/name]
**PRs merged:** N
**PRs still open:** N
**Issues closed:** N
**Issues opened:** N

### Merged this period
- #N "[title]" by @author — merged [date], +X/-Y lines [labels]
- ...

### Open PRs needing attention
- #N "[title]" by @author — [review state], [age]
- ...

### Closed without merge
- #N "[title]" — closed [date], reason: [if available]

### Issues closed
- #N "[title]" — [labels]
- ...

### Issues opened
- #N "[title]" — [labels]
- ...

### Notable review activity
- #N "[title]" — N reviews, [last reviewer]
- ...
```

## Rules

- **Never invent PR numbers or titles.** Only report what `gh` returns.
- **Combine cleanly** with `ingest-from-git` output — the downstream `generate-*` skill receives both as one fact base.
- **Handle rate limits gracefully**: if `gh` returns a rate-limit error, report what was fetched and note the truncation.
- **Skip empty sections** — if no issues were closed, don't write "Issues closed: (none)" repeatedly. Just omit the section.
- **Don't include reviewer names** unless explicitly asked — keeps the output safer to share.

## Pipeline position

```
ingest-from-git + ingest-from-github
                 ↓
        [classify-manager-style]
                 ↓
              generate-X-report
                 ↓
        [format-as-html | format-as-slides]
```
