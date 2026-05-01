---
name: generate-volatile-priority-update
description: >
  Generates an alignment-first update for a volatile-priority manager — someone who frequently
  changes direction mid-sprint, introduces new priorities that override previous agreements, and
  doesn't always connect new requests to existing work in flight. Use this skill after running
  useless-report:classify-manager-style and getting a volatile_priority profile, or directly when
  your manager often says "actually, forget what I said last week" or redirects work without
  acknowledging the change. Also triggers when the user needs to show what was originally agreed,
  what changed, and how the team adapted — without expressing frustration.
---

# generate-volatile-priority-update

## Goal

The volatile-priority manager changes direction frequently — often for legitimate external reasons they haven't communicated fully. The update's job is not to passive-aggressively document every pivot. It is to **create a stable alignment checkpoint**: here's what we understood the priorities to be, here's what changed, here's what we're doing about it, here's what we need to confirm.

This report protects the team (by documenting direction changes) and serves the manager (by giving them a clear picture of current alignment).

No "but you said last week" energy. Matter-of-fact, neutral, actionable.

## Reference

Read `../../archetypes/volatile-priority.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- Summary of work planned vs. work done
- Any direction changes that occurred this week
- Work that was paused, cancelled, or reprioritized
- New priorities introduced mid-week
- Current state of the work

## Output structure

```
## Weekly Alignment Update — [Period]

### 1. Current priority alignment
[What the team understood the priorities to be at the start of the period — stated neutrally]

### 2. What happened
[What was actually worked on, including any pivots — stated factually, no editorializing]

### 3. Adjustments made
[If priorities changed mid-week: what was paused, what was started instead, and what was the understood reason]

### 4. Open questions for alignment
[Questions that need a clear answer before next week's work can be planned confidently]
  - Should we continue X or is Y now the priority?
  - [Any other direction-sensitive question]

### 5. Upcoming work — pending confirmation
[What the team plans to work on next, flagged as "pending alignment confirmation" where relevant]
```

## Rules

- Document direction changes factually — no passive aggression, no "as you requested last week", no frustration in the phrasing.
- The "Open questions for alignment" section is the most important. This creates a paper trail and prompts explicit answers.
- If nothing changed this week, say so: "Priorities were stable this week. No adjustments required."
- Mark upcoming work as "pending confirmation" only if there's genuine uncertainty about direction — don't over-hedge stable work.
- The goal is not to catch them out. It is to help them stay consistent.

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

Keep the "What happened" section factual and brief. The value is in the alignment questions, not the activity log.
