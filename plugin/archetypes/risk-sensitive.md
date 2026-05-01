# Risk-sensitive

**Funny alias**: "Are we sure about this?"

## Observable signals
- Focuses on what could go wrong before acknowledging what went well
- Asks "what's the rollback plan?" or "what happens if this fails?"
- References past incidents, outages, or close calls
- Needs reassurance before approving changes in production or customer-facing areas
- Escalates or asks more questions when anything is described as uncertain
- Uses phrases like "I'm a bit worried about...", "have we stress-tested...", "what's our contingency?"
- Slows decisions when risk is mentioned; speeds up when stability is confirmed

## Underlying need
Safety and the confidence that someone has thought through the failure modes — not pessimism, but a desire to feel prepared before committing.

## Communication strategy
- Detail level: **medium** — focus on risks and mitigations, not exhaustive activity logs
- Update frequency: **weekly, or immediately if something changes risk posture**
- Format preference: risk-first structure — state stability first, then progress, then risks and mitigations
- Tone: measured, calm, reassuring without being dismissive

## Avoid
- Mentioning a risk without immediately following it with a mitigation
- Using words like "uncertain", "might", "we'll see" without context
- Presenting progress without acknowledging the risks involved
- Surprising them with an incident after the fact
- Being defensive when they ask about what could go wrong

## Best output format
`useless-report:generate-risk-report`

Report sections: Status banner (stable / at risk) → Progress → Risks → Mitigations → Rollback plan → Next review

## Example trigger messages
- "Are we sure this is safe to deploy on Friday?"
- "What's the rollback plan if this causes issues?"
- "I'm worried about the migration — did we test edge cases?"
- "Can we wait until next sprint to be safe?"
- "What's our contingency if the API is down?"
