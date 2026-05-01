---
name: generate-synchronous-first-update
description: >
  Generates a pre-meeting brief for a synchronous-first manager — someone who prefers live
  conversations over written updates, responds to messages by scheduling calls, and makes better
  decisions in real-time dialogue than async reading. Use this skill after running
  useless-report:classify-manager-style and getting a synchronous_first profile, or directly when
  your manager always says "let's jump on a call" instead of replying in writing. Also triggers
  when the user needs to prepare for a one-on-one, send a pre-meeting brief that will actually be
  read, or communicate something important to a manager who doesn't engage with long written
  updates.
---

# generate-synchronous-first-update

## Goal

The synchronous-first manager doesn't ignore written communication on purpose — they just process information better in conversation. The written update's job is not to replace the conversation: it is to **make the conversation more efficient**. Write something they can scan in 60 seconds before a call and feel prepared for.

> This is a briefing document, not a report.

## Reference

Read `../../archetypes/synchronous-first.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- What needs to be discussed in the upcoming meeting
- Decisions that need to be made (by them, in the call)
- Context they'll need to make those decisions
- What you need from them
- Any background detail they may want to read after the call

## Output structure

```
## Pre-meeting Brief — [Date / Meeting name]

### The one thing to know before we talk
[Single sentence: the most important context for the meeting]

### Decisions to make in this meeting
[List of decisions they need to make — keep to 3 or fewer]
1. [Decision]: [context in one sentence]
2. [Decision]: [context in one sentence]

### Context (30-second version)
[3-5 bullet points of essential background — no more than needed to make the decisions above]

### What I need from you
[What will you take away from this meeting to move forward]

### Background (optional — for after the call)
[If there are details they might want to read after: put them here. This section is collapsible — they probably won't read it before.]
```

## Rules

- Total document: fits on one screen. If it's longer, the brief will not be read before the call.
- The "Decisions to make" section is the heart of the document. Everything else supports it.
- "Context" should be the minimum needed to make informed decisions — not a full status update.
- The "Background" section is for after the meeting, not before. Don't expect it to be read.
- If you need a decision that requires them to read something long, give them the summary and have the detail available if asked.

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

This report is the exception where **shorter is always better**. They're going to talk to you anyway. The brief just needs to give them enough to start the conversation informed.
