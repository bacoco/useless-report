---
name: classify-manager-style
description: >
  Analyzes a manager's communication style from messages, emails, Slack threads, PR comments,
  or meeting notes and outputs a structured communication profile: archetype name, confidence
  score, observed evidence, recommended communication strategy, and what to avoid. Use this skill
  when the user pastes manager messages and wants to adapt their communication, when preparing a
  status report that needs to land well, when starting the useless-report workflow, or any time the
  user asks "how should I write this for my manager", "what kind of manager is this", or "how does
  my boss communicate". Also triggers on "my manager always asks for...", "my boss never reads...",
  "my manager wants everything in meetings", or similar complaints about upward communication
  friction. This is the entry point for the entire useless-report workflow.
---

# classify-manager-style

## What this skill does

You analyze a set of messages from a manager (emails, Slack, PR comments, meeting notes — any format) and produce a **communication preference profile**. This is not a psychological diagnosis. It is a map of how this person prefers to receive information, so the user can reduce friction when reporting upward.

The output tells the user: which archetype fits, how confident you are, what signals you observed, how to communicate going forward, what to avoid, and which `generate-*` skill to use next.

## The 10 archetypes

Each manager tends toward one dominant pattern, sometimes two. Brief key signal for each:

| Archetype | Key signal | Funny alias |
|---|---|---|
| `control_oriented` | Requests detailed breakdowns; asks to review before execution | "Can you send me a quick detailed breakdown?" |
| `risk_sensitive` | Focuses on what could go wrong; needs reassurance | "Are we sure about this?" |
| `process_heavy` | Wants everything tracked; references procedures | "Is this in the tracking system?" |
| `stakeholder_oriented` | Frames everything in terms of external perception | "How does this look to the steering committee?" |
| `low_context` | Often catches up late; doesn't retain history | "Sorry, catching up — what did we decide?" |
| `volatile_priority` | Frequently changes direction; reprioritizes mid-sprint | "Actually, forget what I said last week" |
| `deadline_reactive` | Urgency-first; time pressure drives all decisions | "We need this by tomorrow, right?" |
| `quality_maximalist` | Asks about edge cases, coverage, completeness | "Did we consider all the edge cases?" |
| `ambiguity_tolerant` | Short approvals; delegates fully; rarely asks for detail | "Yeah just run with it" |
| `synchronous_first` | Prefers calls over written updates; avoids async decisions | "Let's jump on a call about this" |

## Observable signals to score

Read the provided messages and score each archetype on these dimensions:

- **Detail demand**: Does this person ask for breakdowns, logs, or evidence? → `control_oriented`, `process_heavy`, `quality_maximalist`
- **Decision involvement**: Do they want to validate or approve before execution? → `control_oriented`, `stakeholder_oriented`
- **Risk focus**: Do they surface risks, downsides, or "what if" scenarios? → `risk_sensitive`
- **Process attachment**: Do they reference tickets, procedures, compliance, or tracking? → `process_heavy`
- **Deadline pressure**: Do they impose timelines, escalate urgency, reference delivery dates? → `deadline_reactive`
- **Sync preference**: Do they suggest calls, ask for meetings, prefer voice over text? → `synchronous_first`
- **Context gaps**: Do they often ask "remind me", "what did we decide", "catch me up"? → `low_context`
- **Priority shifts**: Do they reprioritize frequently or contradict prior instructions? → `volatile_priority`
- **Brevity / trust signals**: Do they give short approvals, say "just go for it", delegate completely? → `ambiguity_tolerant`
- **Quality focus**: Do they ask about coverage, completeness, edge cases, or test status? → `quality_maximalist`

When two archetypes are close, say so — a manager can be both `control_oriented` and `risk_sensitive`.

## Output format

Always produce both a JSON block and a plain-language summary.

**JSON block:**
```json
{
  "profile": "control_oriented",
  "confidence": 0.82,
  "secondary_profile": "risk_sensitive",
  "secondary_confidence": 0.51,
  "aliases": ["micromanager", "\"Can you send me a quick breakdown?\""],
  "evidence": [
    "Asked for detailed breakdowns in 3 out of 5 messages",
    "Questioned a decision made without prior alignment",
    "Requested options before committing to a direction"
  ],
  "communication_strategy": {
    "detail_level": "high",
    "update_frequency": "frequent (weekly minimum, daily if anything changes)",
    "format": "structured report with options and recommendation",
    "tone": "calm, explicit, non-defensive"
  },
  "avoid": [
    "Surprise decisions — always surface decisions before executing",
    "Vague summaries — specifics reduce follow-up questions",
    "Unexplained tradeoffs — show the reasoning, not just the outcome"
  ],
  "next_skill": "useless-report:generate-control-report"
}
```

**Plain-language summary:** After the JSON, add 2-3 sentences in plain language explaining the profile and the recommended approach. Keep it actionable, not clinical.

## Rules

- Classify only from the evidence provided. If the sample is too small (fewer than 3 messages), say so and state your confidence is low.
- Never invent signals. If a dimension is absent from the messages, don't score it.
- When uncertain between two archetypes, include both with separate confidence scores.
- Always suggest a `next_skill` — the most appropriate `useless-report:generate-*` skill for this profile.
- If the messages suggest a genuinely unusual style not well captured by any archetype, say so.

## The golden rule

> A profile is not a diagnosis. It is a communication preference map.

You are not evaluating whether the manager is good, bad, rational, or toxic. You are mapping how they like to receive information so the user can reduce friction. Keep the output professional and actionable.
