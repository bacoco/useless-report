# Low-context

**Funny alias**: "Sorry, catching up — what did we decide?"

## Observable signals
- Frequently asks "what did we decide on X?" for things already discussed
- Missing from Slack threads and then asking for summaries afterward
- Requests catch-up recaps before meetings
- Asks the same question multiple times across different conversations
- Often jumps into discussions mid-thread without full context
- Uses phrases like "remind me", "quick recap?", "I must have missed this"
- Doesn't retain history well between sessions — treat each update as standalone

## Underlying need
A complete, self-contained picture every time — they don't have the bandwidth or context to connect dots across threads, meetings, and documents.

## Communication strategy
- Detail level: **medium, but self-contained** — always include background, never assume they remember
- Update frequency: **weekly, standalone — each update should be readable without prior context**
- Format preference: TL;DR first, then supporting context; all links explicit; no "as discussed" references
- Tone: patient, clear, no assumptions about what they recall

## Avoid
- "As we discussed..." or "following up from last week..."
- Assuming they read the previous update
- References without context ("BILL-77 is blocked" without saying what BILL-77 is)
- Dense updates that require knowing the history to interpret
- One-liners that need decoding

## Best output format
`useless-report:generate-low-context-digest`

Report sections: TL;DR (3 sentences max) → Context (what is this about) → What happened → What's next → What I need from you

## Example trigger messages
- "Sorry, catching up — what did we decide on the auth flow?"
- "Can you give me a quick recap before the meeting?"
- "I must have missed this — what's the current status?"
- "Remind me, where are we on the migration?"
- "I'm not fully up to speed — can you summarize?"
