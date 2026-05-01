---
name: generate-process-report
description: >
  Generates a formal, fully-traced weekly report for a process-heavy manager — someone who wants
  everything tracked, documented, and cross-referenced with tickets or systems of record. Use this
  skill after running useless-report:classify-manager-style and getting a process_heavy profile, or
  directly when your manager asks "is this in JIRA?", "can you document this?", or "I need this
  for the quarterly review". Also triggers when the user needs a traceability-heavy report, an
  audit-ready summary, or a formal activity log with ticket references.
---

# generate-process-report

## Goal

The process-heavy manager needs to be able to answer "what happened and where is the evidence?" at any time. This report is designed to be the written record — long, structured, traceable. The goal is not to impress them with length, but to give them something they can file, forward, or reference without needing to ask follow-up questions.

> This is the report you write so the audit goes smoothly.

## Reference

Read `../../archetypes/process-heavy.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- Git commits, PR list, ticket list
- Ticket IDs and their status (opened, closed, in progress)
- Decisions logged (or that should be logged)
- Dependencies on other teams or systems
- Pending confirmations or approvals still awaited
- Any process exceptions that occurred (and why they were justified)

## Output structure

```
## Weekly Activity Report — [Period]

### 1. Scope
Period covered: [dates]
Main areas of activity: [2-3 areas]
Team involved: [if relevant]

### 2. Summary
[3-4 sentences: what the week accomplished and its significance]

### 3. Activity by workstream

#### [Workstream A]
- [Activity 1] — Ticket: [ID or "no ticket"]
- [Activity 2] — Ticket: [ID or "no ticket"]

#### [Workstream B]
- [Activity 1] — Ticket: [ID or "no ticket"]

### 4. Ticket-level progress
| Ticket | Title | Status | Notes |
|--------|-------|--------|-------|
| [ID]   | [title] | Completed / In progress / Blocked | [note] |

### 5. Decisions
| Decision | Rationale | Date | Who decided |
|----------|-----------|------|-------------|
| [decision] | [why] | [date] | [who] |

### 6. Dependencies
[External blockers or dependencies on other teams, systems, or processes]

### 7. Pending confirmations
[Items awaiting approval, response, or documentation from others]

### 8. Appendix
[Any reference links, PR URLs, or additional documentation]
```

## Rules

- If no ticket IDs were provided, note "no ticket reference available" in the table rather than omitting the row.
- Decisions should always have a rationale — even a brief one.
- Use tables where possible — they scan faster than paragraphs for this archetype.
- Include the "Pending confirmations" section even if empty: "None pending this week." Absence of content is still content here.
- This report can be long. Length is not a flaw for this archetype.

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

For this archetype, thoroughness IS the reassurance. Expand freely — but only from facts provided. Never invent ticket IDs or decisions.
