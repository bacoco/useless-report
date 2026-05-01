---
name: generate-stakeholder-brief
description: >
  Generates a polished, forward-looking brief for a stakeholder-oriented manager — someone who
  frames everything in terms of external perception, needs material they can forward or present,
  and asks for "talking points" and "key messages" rather than technical details. Use this skill
  after running useless-report:classify-manager-style and getting a stakeholder_oriented profile, or
  directly when your manager asks for a summary they can share with a VP or steering committee.
  Also triggers when the user needs to write a management update that will be forwarded, presented,
  or used in a meeting they won't attend.
---

# generate-stakeholder-brief

## Goal

The stakeholder-oriented manager is a relay — they take your update and transmit it upward or outward. They need material that survives that transmission without modification. The brief must be:
- Readable to someone with no technical context
- Positively framed without being dishonest
- Structured around impact and narrative, not implementation
- Copy-pasteable into a slide or email without editing

Give them sentences they can use, not paragraphs they have to summarize.

## Reference

Read `../../archetypes/stakeholder-oriented.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- Summary of work done this week
- Decisions made
- Upcoming milestones
- Any context about who will receive this (VP, client, committee)
- Any political context the user mentions (tensions, dependencies, negotiations)

## Output structure

```
## Stakeholder Update — [Period]

### Key impact
[3 bullet points: what moved, why it matters to the business or users — no technical jargon]

### Decisions
[Each decision framed as: We chose [X]. This [benefits / protects / accelerates] [Y].]

### Talking points
[3-5 sentences the manager can use verbatim in a conversation or meeting]
  - "[Talking point 1]"
  - "[Talking point 2]"
  - "[Talking point 3]"

### Upcoming milestones
[What's coming next, framed as momentum — not as "we'll see if it works"]

### If asked about [risk or concern]
[Optional: pre-emptive framing for a likely question — "If someone asks about the delay, the answer is: ..."]
```

## Rules

- No technical jargon. If a technical term must appear, define it in the same sentence.
- Frame everything around impact on users, customers, or business goals — not implementation details.
- Talking points should be complete sentences the manager can say out loud. Not bullets they have to expand.
- If there's bad news, include it — but with framing. "The migration is delayed by two weeks. We're using the time to add safety checks that reduce risk at launch." Honest, but shaped.
- The "If asked about" section is optional but powerful — anticipate the awkward question and give them an answer.

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

This is the shortest report format. Cut every sentence that requires technical context to understand. What remains should be usable without editing.
