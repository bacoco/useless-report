---
name: generate-control-report
description: >
  Generates a detailed, structured weekly report for a control-oriented manager — the kind who
  asks for breakdowns, wants to review decisions before they're executed, and prefers options with
  explicit recommendations. Use this skill after running useless-report:classify-manager-style and
  getting a control_oriented profile, or directly when you know your manager fits the "Can you send
  me a quick detailed breakdown?" pattern. Also triggers when the user wants to write a weekly status
  update for a micromanager, when they want to preempt follow-up questions, or when they say things
  like "my manager always wants details" or "my boss needs to approve everything".
---

# generate-control-report

## Goal

The control-oriented manager feels uncomfortable when they can't see the details. This report's job is to pre-pay the cost of their follow-up questions by including the information they would have asked for anyway. A good control report reduces interruptions by giving visibility upfront.

> Spend 5 minutes generating this. Save 2 hours of follow-up questions.

This is not about overwhelming them with noise. It is about **controlled verbosity**: expand from facts, add explicit rationale, surface options, show traceability.

## Reference

Read `../../archetypes/control-oriented.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- Git log or commit summary for the period
- PR list (titles, status, linked tickets)
- Ticket/issue list (completed, in progress, blocked)
- Any decisions made during the period
- Blockers or risks
- A manual prose summary of what was done

If the user provides raw git log output, extract the meaningful work from it — don't just dump commit messages.

## Output structure

Generate a report with exactly these sections, in this order:

```
## Weekly Control Report — [Period]

### 1. Summary
[2-3 sentences: what the week was about, overall status]

### 2. Completed work
[Bullet list with brief rationale for each item — not just what, but why it mattered]

### 3. Decisions made
[Each decision as: Decision: [what]. Reason: [why.] Impact: [consequence of this choice.]]

### 4. Options considered
[For any significant decision, list the options that were on the table]
  Option A — [name]: [pros / cons]
  Option B — [name]: [pros / cons]
  Chosen: [which one and why]

### 5. Risks
[Active risks with current status and mitigation in place]

### 6. What I need from you
[Explicit asks — approvals, unblocks, decisions they need to make]

### 7. Traceability
[Ticket IDs, PR links, or reference numbers for completed items]
```

## Rules

- Expand only from facts provided. Never fabricate completed work, decisions, or risks.
- If options were not explicitly provided by the user, note what was considered (even if briefly) rather than inventing alternatives.
- Keep the tone calm and non-defensive — even if a decision was made under pressure, present it neutrally.
- The "What I need from you" section is important: control-oriented managers respond well to explicit asks. If there's nothing to ask, say so.
- Traceability is not optional. If no ticket IDs were given, note "no ticket references provided".

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

Every extra sentence should answer a question the manager would have asked anyway. If it doesn't, cut it.
