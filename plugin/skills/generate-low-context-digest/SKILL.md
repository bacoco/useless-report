---
name: generate-low-context-digest
description: >
  Generates a fully self-contained update for a low-context manager — someone who frequently misses
  threads, asks for recaps, and can't be assumed to remember what was discussed last week. Use this
  skill after running useless-report:classify-manager-style and getting a low_context profile, or
  directly when your manager often says "remind me", "what did we decide?", or "I must have missed
  this". Also triggers when the user needs to write an update that should be readable by someone
  with no prior context, or when they're updating a manager who has been out or is juggling too much.
---

# generate-low-context-digest

## Goal

The low-context manager is not disengaged — they're overloaded. Every update they receive competes with dozens of others. Write as if they have no memory of prior conversations: no "as we discussed", no "following up from last week", no references that require decoding.

> Each update is a standalone document. Treat it that way.

The goal is a digest they can read in 2 minutes and leave knowing exactly what's happening without needing to read anything else.

## Reference

Read `../../archetypes/low-context.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- Summary of work done this week
- Any decisions made (with full context, not just "we chose option B")
- Blockers or dependencies
- What's coming next
- What is needed from them

## Output structure

```
## Weekly Digest — [Period]

### TL;DR
[3 sentences max: what this period was about, overall status, one thing to know]

### Context
[What is this project/work? 2-3 sentences that would orient a new reader. Assume nothing.]

### What happened
[Bullet list of what was done — each item self-contained, no shorthand]

### What's next
[What the team will focus on next, in plain terms]

### What I need from you
[Explicit asks — decision needed, approval, unblock — or "Nothing needed this week."]
```

## Rules

- Never use "as discussed", "following up", "as mentioned", or any reference that assumes prior context.
- Every ticket ID or technical term that appears must be explained: "AUTH-184 (the session expiry bug)" not just "AUTH-184".
- The TL;DR must be readable by someone who will read nothing else. If the TL;DR is accurate and complete, the update is working.
- If there is nothing needed from them, say so explicitly — "Nothing needed this week." They shouldn't have to wonder.
- "What happened" bullets should be written so a new reader understands the significance, not just the fact.

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

Clarity beats brevity here. A slightly longer sentence that removes ambiguity is better than a short sentence that requires prior context to interpret.
