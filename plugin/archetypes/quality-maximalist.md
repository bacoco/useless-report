# Quality-maximalist

**Funny alias**: "Did we consider all the edge cases?"

## Observable signals
- Asks about test coverage, edge cases, and error handling before approving work
- Reads PRs carefully and leaves detailed review comments
- Hesitates to ship when test coverage feels incomplete
- Asks "what happens if X fails?" or "did we test the Y scenario?"
- References past bugs or incidents as justification for thoroughness
- Uses phrases like "I want to be confident this is solid", "did we cover the unhappy paths?", "what's our rollback?"
- Approval often comes with a list of clarifying questions about completeness

## Underlying need
Confidence that the work is genuinely correct and won't cause problems after delivery — quality is not perfectionism for its own sake, it's risk management.

## Communication strategy
- Detail level: **high on quality signals** — show what was tested, what gaps remain, and why they're acceptable
- Update frequency: **weekly, or at milestones**
- Format preference: quality-first structure — coverage, edge cases addressed, known gaps with rationale
- Tone: precise, evidence-based, honest about gaps

## Avoid
- "It's working fine" without supporting evidence
- Glossing over known limitations without acknowledging them
- Shipping without communicating what was and wasn't tested
- Reacting defensively to quality-focused questions
- Promising thoroughness without specifics

## Best output format
`useless-report:generate-quality-maximalist-report`

Report sections: Quality coverage → Edge cases addressed → Known gaps (with rationale for why they're acceptable) → Test status → Open risks → Next quality steps

## Example trigger messages
- "Did we test the edge cases for the auth flow?"
- "What's our test coverage on this?"
- "I want to be confident this won't cause issues in production."
- "Did we consider what happens when the API is unavailable?"
- "Can we add a few more tests before we ship this?"
