---
name: track-history
description: >
  Persists every generated report to a local history store, tracks manager requests and TODOs
  across sessions, and generates a delta section comparing current work to previous commitments.
  Answers: "what did I promise last time?", "what's still open?", "what changed since my last
  report?". Runs automatically as part of weekly-report. Also invokable standalone to review
  history, list open manager requests, or generate a delta summary. Use when the user asks
  "what was outstanding from last week?", "what did my manager ask for?", "show me the diff
  since last report", or "what's still on my TODO list from Alice?".
---

# track-history

## Goal

Never lose track of what was said, what was promised, and what was delivered. Build a persistent memory of reports, manager requests, and commitments — so every new report starts from where the last one ended.

> Every report knows what came before it.

## Storage structure

All history lives in `.useless-report/history/` at the project root:

```
.useless-report/
  config.yml                        ← profiles (managers + user persona)
  history/
    index.json                      ← chronological list of all reports
    2026-05-01/
      alice-control.md              ← report snapshot (markdown)
      alice-requests.json           ← manager requests extracted from this report
      delta.md                      ← delta vs previous report (auto-generated)
    2026-04-24/
      alice-control.md
      alice-requests.json
      delta.md
  open-requests.json                ← aggregated list of all open manager requests
```

## How to use

### Step 1 — On every weekly-report run (automatic)

**Save the current report:**
```json
// history/index.json — append entry
{
  "date": "2026-05-01",
  "manager": "alice",
  "profile": "control_oriented",
  "reportPath": "history/2026-05-01/alice-control.md",
  "requestsPath": "history/2026-05-01/alice-requests.json"
}
```

**Extract manager requests** from the report's "What I need from you" / "Asks" / "Decisions needed" sections:

```json
// history/2026-05-01/alice-requests.json
{
  "date": "2026-05-01",
  "manager": "alice",
  "requests": [
    {
      "id": "req-20260501-001",
      "text": "Approve the migration plan for AUTH module",
      "type": "decision",         // decision | information | action | approval
      "status": "open",           // open | resolved | dropped
      "resolvedDate": null,
      "resolvedIn": null          // path to report where resolved
    },
    {
      "id": "req-20260501-002",
      "text": "Send breakdown of Q2 delivery risk",
      "type": "information",
      "status": "open",
      "resolvedDate": null,
      "resolvedIn": null
    }
  ]
}
```

**Update open-requests.json** — merge new requests, mark previously open ones as resolved if they appear in the current report's "resolved" or "delivered" sections.

### Step 2 — Generate the delta

Compare current report to the most recent previous report for the same manager:

```markdown
## Since last time (2026-04-24 → 2026-05-01)

### ✅ Delivered (was in progress last week)
- AUTH migration plan — shipped to staging
- Q2 delivery risk breakdown — sent to Alice on Apr 28

### 🔄 Still in progress
- Billing retry logic — 60% done, on track for May 8

### 🆕 New this week
- Invoice PDF export — started, first version by May 6
- API v2 documentation — blocked on design review

### ⚠️ Open requests from Alice (not yet resolved)
- [ ] Decision on AUTH module architecture (requested 2026-04-24)
- [ ] Review of Q3 roadmap draft (requested 2026-04-17)
```

Save to `history/[date]/delta.md`. Prepend to the main report when generating HTML/slides/email.

### Step 3 — Standalone invocation

When called directly (`/useless-report:track-history`), show a menu:

```
What would you like to see?
1. Open requests from [manager] (unresolved)
2. Delta since last report
3. Full history for [manager]
4. All open requests (all managers)
5. Mark a request as resolved
```

**Option 1 — Open requests:**
List all unresolved manager requests, sorted by age. Flag anything older than 2 weeks as ⚠️.

**Option 3 — Full history:**
Show a timeline of all reports for the manager, with key metrics per period
(commits, PRs, tickets, status at time of report).

**Option 5 — Mark resolved:**
Interactively update `open-requests.json`. Ask which request, mark resolved, save.

### Step 4 — Auto-detect resolved requests

When generating a new report, scan the new content for mentions of previously open requests:
- If a previous request appears in "delivered" or "done" → auto-mark as resolved
- If a previously flagged risk has been mitigated → note it in delta
- If a previous blocker is gone → surface it as a win

## Config integration

`track-history` reads the `managers` list from `config.yml` to know which managers to track separately. Each manager has their own history thread.

## Rules

- **Never delete history.** Only append. A dropped request is marked `"status": "dropped"` with a note, never removed.
- **Never fabricate past reports.** History only contains what was actually generated by useless-report.
- **Requests are extracted conservatively.** Only tag something as a "manager request" if it appears in a section explicitly asking for action/decision from the manager.
- **Delta is factual.** Compare report content directly — don't editorialize about "improvement" or "regression" unless numbers support it.
- **Privacy.** History files stay local — never sent anywhere. They're in the project's gitignore by default (add `.useless-report/history/` to `.gitignore`).

## Pipeline position

```
weekly-report
      ↓
  track-history  ← runs automatically (save + extract + delta)
      ↓
  generate-X-report (with delta prepended)
      ↓
  format-as-html / format-as-qa / format-as-call-prep
```

Also callable standalone for reviewing open requests and history.
