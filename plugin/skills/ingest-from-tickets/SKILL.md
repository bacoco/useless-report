---
name: ingest-from-tickets
description: >
  Extracts ticket references (Jira, Linear, GitHub issues) from git commits and PRs in the current
  repository, then optionally enriches each ticket with title and status if the appropriate CLI
  is configured (jira-cli, linear-cli, gh). Use this skill when the user wants their manager
  report to include ticket-level traceability, when their commits use AUTH-184 / BILL-77 / LIN-99
  patterns and they want those expanded into readable summaries, or before generating a process-
  heavy or control-oriented report that benefits from ticket detail. Triggers on "what tickets
  did we close", "expand the JIRA references", "show me the Linear items".
---

# ingest-from-tickets

## Goal

Find ticket references in git history and PR activity, then enrich them with titles and status where possible. Output a deduplicated list of tickets the manager can verify or click through.

## How to use

### Step 1 — Find ticket IDs

Scan the output of `ingest-from-git` and `ingest-from-github` (or run fresh git commands) and extract ticket-like patterns:

- **Jira-style**: `[A-Z]{2,10}-\d+` (e.g., `AUTH-184`, `PLATFORM-1209`, `BILLING-77`)
- **Linear-style**: same pattern, often shorter prefixes (e.g., `LIN-99`, `ENG-42`)
- **GitHub issues**: `#\d+` references in commit messages or PR descriptions

```bash
# From git commit messages
git log --since="<period>" --pretty=format:"%s%n%b" \
  | grep -oE '[A-Z]{2,10}-[0-9]+' | sort -u

# From GitHub issue references in commits
git log --since="<period>" --pretty=format:"%s%n%b" \
  | grep -oE '#[0-9]+' | sort -u
```

Deduplicate the result.

### Step 2 — Enrich (optional, only if CLI available)

Try each enrichment source. **Skip silently** if the CLI is missing — never fail the whole skill because Linear isn't installed.

**For GitHub issues** (`gh` CLI):
```bash
gh issue view <N> --json number,title,state,labels,assignees
```

**For Jira** (only if `jira` CLI configured):
```bash
jira issue view <KEY>
```

**For Linear** (only if `linear-cli` configured):
```bash
linear issue <ID>
```

If no CLI is available for a ticket type, list the IDs without enrichment and note: *"Titles unavailable — install jira-cli/linear-cli or paste titles manually for richer output."*

### Step 3 — Output

```md
## Ticket References — [start] to [end]

**Total unique tickets referenced:** N

### Tickets (enriched where possible)
- AUTH-184 — "Fix session expiry edge case" [DONE]
- BILL-77 — "Refactor billing migration fallback" [IN PROGRESS]
- #129 — "Add retry logic for webhook delivery" [merged]
- ...

### Tickets without details
- PLATFORM-1209 (no enrichment available)
- ...
```

## Rules

- **Never invent ticket titles or status.** If you can't fetch the title, list the ID alone.
- **Deduplicate** before enriching — don't waste API calls on duplicates.
- **Cap at ~30 tickets** in the output by default. If more, group and note "+ N more".
- **Group by status** if enrichment worked: Done / In Progress / Open / Blocked.
- **Report enrichment failures** at the bottom in one line — don't make it noisy.

## Pipeline position

This skill is **optional** in the pipeline. Most reports work fine without it. Use it when the manager profile is `process_heavy` or `control_oriented` (high traceability need), or when the user explicitly asks for ticket-level detail.

```
ingest-from-git + ingest-from-github + ingest-from-tickets
                       ↓
              generate-X-report
```
