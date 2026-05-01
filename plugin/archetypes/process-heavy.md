# Process-heavy

**Funny alias**: "Is this in the tracking system?"

## Observable signals
- References tickets, JIRA issues, Linear items, or Confluence pages in every discussion
- Asks for documentation before action is taken
- Wants decisions logged somewhere permanent
- Uses phrases like "is this tracked?", "where's the ticket?", "can you document this?"
- Follows up if a process step was skipped, even if the outcome was fine
- Values traceability over speed
- Uncomfortable with informal decisions made in Slack that aren't reflected in the official system
- May escalate if procedures weren't followed, regardless of outcome

## Underlying need
Auditability and traceability — confidence that if something goes wrong (or if someone asks), there is a documented record of what happened and why.

## Communication strategy
- Detail level: **high** — include ticket references, dates, who decided what
- Update frequency: **weekly, on schedule**
- Format preference: structured sections with explicit traceability (ticket IDs, links, scope statements)
- Tone: formal, organized, consistent

## Avoid
- Informal updates without ticket references
- "We handled it" without documentation of how
- Skipping sections of the standard report format
- Verbal-only decisions not recorded anywhere
- Responding to process concerns with "but the outcome was fine"

## Best output format
`useless-report:generate-process-report`

Report sections: Scope → Activity by workstream → Ticket-level progress → Decisions → Dependencies → Pending confirmations → Appendix

## Example trigger messages
- "Is this tracked in JIRA?"
- "Can you update the Confluence page before EOD?"
- "I need a written summary for the quarterly review."
- "Was there a ticket for that change?"
- "Please document the decision before we move forward."
