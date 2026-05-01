---
name: generate-risk-report
description: >
  Generates a stability-first weekly update for a risk-sensitive manager — someone who focuses on
  what could go wrong before acknowledging what went right and needs reassurance before approving
  changes. Use this skill after running useless-report:classify-manager-style and getting a
  risk_sensitive profile, or directly when your manager always asks "are we sure about this?",
  "what's the rollback plan?", or "have we stress-tested this?". Also triggers when the user says
  their manager is anxious about production changes, needs a risk-focused update, or always wants
  to know the contingency plan.
---

# generate-risk-report

## Goal

The risk-sensitive manager doesn't distrust the team — they feel responsible for what goes wrong and need to see that someone has thought through the failure modes. This report's job is to **acknowledge the risks first, then show they're managed**. Reassurance must be earned through evidence, not assertion.

Never say "everything is fine" without showing why it's fine.

## Reference

Read `../../archetypes/risk-sensitive.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- Summary of work done this week
- Any changes deployed to production or customer-facing systems
- Known risks or concerns
- Mitigations in place
- Rollback or contingency plan (if applicable)
- Upcoming changes that could affect stability

## Output structure

```
## Weekly Risk & Stability Update — [Period]

### 1. Overall status
[One line: STABLE / MONITORING / AT RISK — then a sentence explaining why]

### 2. Progress
[Brief summary of what moved forward — kept short, this is context not the main event]

### 3. Changes deployed
[What went to production or staging, with impact scope]

### 4. Risks
[Each risk as: Risk: [what]. Likelihood: [low/medium/high]. Impact if triggered: [what happens]. Mitigation: [what's in place.]]

### 5. Mitigations active
[Summary of safety nets: rollback paths, feature flags, monitoring, test coverage]

### 6. Upcoming changes that warrant attention
[What's coming next week that carries risk, and how it will be managed]

### 7. What I need from you
[Explicit asks — approvals for risky changes, decisions about risk tolerance]
```

## Rules

- Always lead with the status banner. Don't make them read to the end to find out if things are okay.
- Every risk must be paired with a mitigation. Listing a risk without a mitigation will increase their anxiety, not reduce it.
- If there are no significant risks, say so explicitly — "No active risks this week. Monitoring [X] as a precaution."
- Never downplay a real risk to seem calm. They will find out, and it will destroy trust.
- Use concrete language: "rollback deployed in 8 minutes in the last incident" is more reassuring than "we have a rollback process".

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

The risk section should be as long as it needs to be. The progress section should be brief. Get to the mitigations fast.
