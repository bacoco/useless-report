---
name: generate-ambiguity-tolerant-update
description: >
  Generates a brief, high-signal update for an ambiguity-tolerant manager — someone who gives
  short approvals, delegates decisions, and doesn't need detail to feel confident. Use this skill
  after running useless-report:classify-manager-style and getting an ambiguity_tolerant profile, or
  directly when your manager always says "yeah just run with it", "lgtm", or "I trust your
  judgment". Also triggers when the user needs a short weekly check-in, a minimal FYI update, or
  wants to avoid over-reporting to a manager who doesn't need the detail.
---

# generate-ambiguity-tolerant-update

## Goal

The ambiguity-tolerant manager hired capable people and expects things to move. Over-reporting wastes their time and can actually reduce trust — it signals that you need more hand-holding than you do. The right update is **short, outcome-focused, and only asks for input when genuinely needed**.

> If they don't need to know it, don't tell them.

## Reference

Read `../../archetypes/ambiguity-tolerant.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- What was done this week (brief)
- Any significant decisions made
- Anything notable they should be aware of (optional)
- Anything that genuinely needs their input (optional — if nothing, say so)

## Output structure

```
## Weekly Update — [Period]

### Summary
[3 sentences max: what moved, overall status, anything notable]

### Key decisions made
[Only decisions they'd want to know about — not routine ones]
- [Decision]: [one sentence rationale]

### FYI
[Optional: things they might want to know but don't need to act on]
- [Item]

### What I need from you
[Only include if genuinely needed. If nothing: "Nothing needed this week — team is moving."]
```

## Rules

- Total report length: aim for half a page or less. If it's longer, cut.
- Only include the FYI section if there's something worth knowing. Omit it if not.
- "What I need from you" should have zero items most weeks. This manager trusts you to handle things.
- Don't pad with activity details they don't need. "Merged 4 PRs and closed 3 tickets" is not useful to this person unless something notable happened.
- If there's nothing notable to report, say: "Quiet week — steady progress, nothing to flag."

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

For this archetype, **less is more**. The report earns trust by showing you know what they care about (outcomes, exceptions) versus what they don't need (process, routine activity).
