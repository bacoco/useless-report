---
name: format-as-call-prep
description: >
  Generates a natural call preparation document — talking points, opening hook, key messages,
  objection responses, and closing ask — adapted to both the manager's archetype and the user's
  own communication persona. Not a script: a structured guide that sounds like you. Covers
  status calls, idea pitches, crisis management, and general check-ins. Use when the user says
  "I have a call with my manager", "help me pitch this idea", "I need talking points",
  "prepare me for this conversation", or "what do I say on this call".
---

# format-as-call-prep

## Goal

Produce a call preparation guide that feels natural to deliver — not a script to read, but structured talking points calibrated to who you are and who you're talking to.

> Sound like yourself. Sound prepared. Sound like it just came to you.

## How to use

### Step 1 — Load inputs

1. The markdown report (current work context)
2. Manager archetype (from `config.yml`)
3. **User persona** (from `config.yml` — critical for this skill):
   - `style`: assertive / diplomatic / data-driven / storyteller / minimalist
   - `tone`: confident / humble / collaborative / direct
   - `strengths`: list of what the user is good at
   - `role`: their job title/context
4. **Call mode** — ask if not specified:
   - `status_update` — regular check-in, weekly review
   - `pitch` — selling an idea, proposing a change, getting buy-in
   - `bad_news` — something went wrong, delay, blocker, missed expectation
   - `escalation` — you need something from them urgently
   - `feedback` — you want to give or request feedback
5. History (`.useless-report/history/`) — what was outstanding from last time

### Step 2 — Build the call structure

Adapt structure to call mode:

**status_update:**
```
Opening (30s) → 3 key points (2min each) → What I need (30s) → Close
```

**pitch:**
```
Hook / why now (30s) → The problem (1min) → My proposal (2min) →
Why it works (1min) → What I need from you (30s) → Handle objections
```

**bad_news:**
```
Lead with the solution, not the problem (15s) → What happened (1min) →
What we're doing about it (2min) → What I need (30s) → Timeline
```

**escalation:**
```
The ask first (15s) → Why it's urgent (1min) → What happens if not resolved (30s) →
Specific action requested (30s)
```

**feedback:**
```
Positive framing → Specific observation → Impact → Request or suggestion
```

### Step 3 — Apply user persona

The same talking points sound different depending on who's delivering them. Adapt language to the user's style:

**assertive** — short declarative sentences, numbers upfront, no hedging
> "We shipped X. It works. Here's the data." ← not → "I think we did pretty well with X..."

**diplomatic** — acknowledge context, frame contributions collectively, soften asks
> "The team made strong progress on X, and we'd benefit from your input on Y."

**data-driven** — lead with metrics, reference specifics, use percentages and dates
> "Coverage is at 87%. Latency dropped 40ms. Three blockers resolved in 4 days."

**storyteller** — narrative arc, before/after, human context
> "Last week we were stuck on X. We tried two approaches. Here's what worked and why."

**minimalist** — fewest words possible, no filler, headers only if needed
> "Done: X, Y, Z. Blocker: A. Need: decision on B by Friday."

### Step 4 — Adapt to manager archetype

Same content, different framing per archetype:

- **control_oriented**: give them the detail first, then the summary. Offer alternatives.
- **risk_sensitive**: lead with what's safe, then address risks head-on before they ask.
- **process_heavy**: reference the ticket, the standup, the process. Name the system.
- **stakeholder_oriented**: frame everything as narrative. "The story here is…"
- **low_context**: shorter. Simpler. Assume nothing. Offer to send a recap after.
- **volatile_priority**: be ready to pivot. Hold key points loosely. Confirm priorities first.
- **deadline_reactive**: dates before details. "Done by X" before "how".
- **quality_maximalist**: address edge cases unprompted. Show you've thought it through.
- **ambiguity_tolerant**: bring direction. They want you to propose, not ask.
- **synchronous_first**: warm open, conversational, leave space for them to talk.

### Step 5 — Generate the document

Output structure:

```
━━━ CALL PREP — [Manager name] · [Call mode] · [Date] ━━━━━━━━━━━━━━━━━━━━

⏱ BEFORE YOU DIAL
  □ Open the report tab (you don't need to read it — just have it)
  □ One number to remember: [single most important metric]
  □ Your ask in one sentence: [what you need from this call]

━━━ OPENING (30 seconds) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Opening hook adapted to user style + call mode]

Example: "Quick one — three things, then I'll need 30 seconds from you."
or: "Good news first: [win]. Then I want to flag one thing."

━━━ KEY POINTS (say these, in this order) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

① [First point — adapted to archetype]
   → [Supporting detail / number]
   → [Why it matters to them specifically]

② [Second point]
   → [Supporting detail]
   → [Implication]

③ [Third point — often the most important or sensitive]
   → [Supporting detail]
   → [What you need from them, if anything]

━━━ YOUR ASK ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Be specific. One ask per call.

[Exact phrasing of your ask, in your style]

Example (assertive): "I need a decision on X by tomorrow EOD."
Example (diplomatic): "It would really help us move faster if you could weigh in on X before our next standup."

━━━ IF THEY PUSH BACK ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Likely objection 1]: "[How to respond — from data in the report]"
[Likely objection 2]: "[How to respond]"
⚠️ [Danger topic — likely to derail]: "[Strategy: acknowledge, redirect, follow up]"

━━━ CLOSE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[One sentence close adapted to tone]
+ Offer to send a written summary after ("I'll drop you a note with the key points")

━━━ IF IT GOES SIDEWAYS ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[2–3 phrases to use if the conversation gets difficult, based on their archetype]
"Let me get back to you on that with specifics."
"That's a fair point — can I think about it and follow up?"
"I want to make sure I give you the right answer on this."
```

### Step 6 — Save

Save to `./useless-report/[date]/[manager-name]-call-prep.md` (markdown — readable anywhere, phone-friendly).

Confirm:
```
Call prep prêt → useless-report/2026-05-01/alice-call-prep.md
Mode : status_update · Style : assertive · Archétype : control_oriented
```

## User persona in config.yml

The skill reads and writes the user section of `.useless-report/config.yml`:

```yaml
user:
  name: "[Prénom]"
  role: "[Titre / contexte]"
  style: assertive          # assertive | diplomatic | data-driven | storyteller | minimalist
  tone: confident           # confident | humble | collaborative | direct
  strengths:
    - "delivery track record"
    - "technical depth"
    - "proactive risk management"

  # Modes de présentation par contexte
  modes:
    status_update: "data-first, let numbers speak, offer options"
    pitch:         "hook → problem → solution → ask, acknowledge concerns"
    bad_news:      "solution-first, context second, timeline always"
    escalation:    "ask-first, urgency clear, specific action requested"
    feedback:      "observation → impact → request, no blame"
```

If this section doesn't exist in `config.yml`, ask the user to fill it in once and save it. Never ask again.

## Rules

- **Sound like the user, not like a report.** Adapt language to their style — don't generate corporate boilerplate.
- **One ask per call.** Don't generate a list of asks. Pick the most important one.
- **Ground everything in the report data.** No invented achievements or metrics.
- **Flag the danger topic.** Every call has one thing that could derail it — name it.
- **Mobile-friendly output.** This is read just before or during a call. Keep it scannable.

## Pipeline position

```
generate-X-report + user persona + manager archetype
              ↓
     format-as-call-prep  ← you are here
              ↓
   [manager]-call-prep.md
              ↓
   Read before dialing. Close the deal.
```
