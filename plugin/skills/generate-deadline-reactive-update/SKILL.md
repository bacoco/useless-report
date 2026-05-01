---
name: generate-deadline-reactive-update
description: >
  Generates a deadline-first status update for a deadline-reactive manager — someone who leads
  every conversation with delivery dates, interprets uncertainty as a threat to the timeline, and
  wants to know if the deadline is safe before anything else. Use this skill after running
  useless-report:classify-manager-style and getting a deadline_reactive profile, or directly when
  your manager always asks "are we going to make it?", "what's blocking us?", or "do we need to
  cut scope?". Also triggers when the user needs to write an urgent status update, communicate
  a deadline risk, or provide a delivery-focused progress report.
---

# generate-deadline-reactive-update

## Goal

The deadline-reactive manager has one question: **will this be ready on time?** Everything else is secondary. Answer that question in the first line. Then explain the situation.

If the deadline is at risk, say so immediately and lead with the plan to address it. Never bury bad news about timelines — they will find it disproportionately alarming if it appears mid-paragraph.

## Reference

Read `../../archetypes/deadline-reactive.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- Current delivery status per major deliverable
- What was completed this week
- Blockers (technical, dependency, resource)
- Plan to resolve blockers
- Scope that could be cut if needed
- What is needed to stay on track

## Output structure

```
## Delivery Status Update — [Period]

### Status
[ONE LINE — the most important thing: ON TRACK / AT RISK / BLOCKED for [deadline name]]

### Deadline status
| Deliverable | Deadline | Status | Notes |
|-------------|----------|--------|-------|
| [item]      | [date]   | On track / At risk / Done | [note] |

### What's done
[Brief bullet list — proof of progress]

### Blockers
[Each blocker as: Blocker: [what]. Impact: [which deadline is affected and by how much]. Plan: [how we're addressing it.]]

### What I need to stay on track
[Specific asks — unblocks, decisions, scope trade-offs they need to make]

### Contingency (if AT RISK)
[If a deadline is at risk: Option A (keep scope, accept delay), Option B (cut scope X, meet deadline), Recommendation]
```

## Rules

- The status banner is non-negotiable — it goes first and it must be honest.
- Every blocker must come with a plan. A blocker without a plan is panic. A blocker with a plan is a status update.
- If the deadline is at risk, include the contingency section with explicit options. They will need to make a decision.
- "What I need to stay on track" should be specific and actionable — not "support" but "approval to deprioritize feature X".
- Don't use hedging language: not "should be on track" but "on track" or "at risk — here's why".

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

Lead with the status. Everything else is supporting evidence. Keep "What's done" brief — they care about the future, not the past.
