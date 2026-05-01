# Deadline-reactive

**Funny alias**: "We need this by tomorrow, right?"

## Observable signals
- Every conversation centers on delivery dates
- Asks "is this going to be ready for [date]?" before asking about quality or correctness
- Responds to blockers with "how do we get around it?" rather than "what's causing it?"
- Uses phrases like "we're running out of time", "this needs to ship", "we can fix it after launch"
- Interprets any uncertainty as a threat to the timeline
- May ask for scope cuts to preserve dates
- Urgency is the default mode, not the exception

## Underlying need
On-time delivery and the ability to tell stakeholders that commitments will be met — deadlines represent accountability to people above them.

## Communication strategy
- Detail level: **low to medium** — lead with delivery status, follow with blockers and plan
- Update frequency: **frequent during crunch; weekly otherwise**
- Format preference: status banner first (on track / at risk / blocked), then timeline, then blockers and mitigation
- Tone: direct, confident, action-oriented

## Avoid
- Burying the deadline status in the middle of the report
- Mentioning a blocker without a plan to resolve it
- Using hedging language ("should be fine", "probably on track")
- Detailed technical explanations when they just want a status
- Presenting options when they want a decision

## Best output format
`useless-report:generate-deadline-reactive-update`

Report sections: Status banner (ON TRACK / AT RISK / BLOCKED) → Deadline status per deliverable → What's done → Blockers → Mitigation plan → What I need to stay on track

## Example trigger messages
- "Are we still going to make the Friday deadline?"
- "This needs to ship by end of month — what's the plan?"
- "What's blocking us? Can we work around it?"
- "Do we need to cut scope to meet the date?"
- "I need a status by EOD — are we on track?"
