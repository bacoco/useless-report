---
name: format-as-qa
description: >
  Generates a Q&A preparation document from a manager report — all the questions the manager
  is likely to ask (based on their archetype), with prepared answers derived strictly from the
  report data. Includes "danger questions" (the ones that catch you off guard), objection
  responses, and a quick-reference cheat sheet. Output is a collapsible HTML file to review
  before a meeting or call. Use when the user asks "what will my manager ask?", "help me
  prepare for my meeting", "I have a review tomorrow", or "Q&A prep".
---

# format-as-qa

## Goal

Turn a manager report into a meeting preparation document: every question they're likely to ask, a prepared answer for each, danger zones flagged, and a 30-second cheat sheet at the top.

> Read this once before the meeting. Walk in confident.

## How to use

### Step 1 — Load inputs

1. The markdown report (just generated or provided by the user)
2. The manager's archetype (from `.useless-report/config.yml` or ask)
3. The user's persona (from `config.yml` — style, tone, strengths)
4. Previous history if available (`.useless-report/history/` — what was outstanding)

### Step 2 — Generate questions by archetype

Each archetype has predictable question patterns. Generate 8–15 questions per manager:

**control_oriented** — asks about details, options, decisions
> "Why did you choose X over Y?", "Who approved this?", "What are the alternatives?",
> "Can you walk me through the timeline?", "What's the risk if we delay?",
> "How does this fit with what we decided last month?"

**risk_sensitive** — asks about failure modes, confidence, backup plans
> "What could go wrong?", "How confident are you in this estimate?",
> "What's the fallback if this doesn't work?", "Have we tested edge cases?",
> "What's the worst-case scenario?", "Who else has reviewed this?"

**process_heavy** — asks about compliance, documentation, process
> "Is this in the tracking system?", "Was this reviewed in the standup?",
> "Do we have a ticket for this?", "Is the documentation updated?",
> "Did you follow the process for X?", "What's the sign-off chain?"

**stakeholder_oriented** — asks about visibility, narrative, external impact
> "How does this look to [client/exec]?", "Can we communicate this as a win?",
> "What's the story we tell here?", "Who else needs to know about this?",
> "How does this align with our Q[X] commitments?"

**low_context** — asks for basics, definitions, repetition
> "Wait, what is X again?", "Can you give me the TL;DR?",
> "Which project is this for?", "Who owns this?",
> "Can you send me a summary after?"

**volatile_priority** — asks about pivots, current status vs last conversation
> "Is this still the priority?", "I thought we were doing Y — what changed?",
> "Can we do this faster?", "What if we dropped X to focus on this?"

**deadline_reactive** — asks about dates, blockers, acceleration
> "When exactly will this be done?", "What's blocking you?",
> "Can we pull this in?", "What do you need to hit the date?"

**quality_maximalist** — asks about edge cases, tests, standards
> "Did we test all the edge cases?", "What's the test coverage?",
> "Are there any technical debt implications?", "How does this affect performance?"

**ambiguity_tolerant** — asks for direction, priorities, big picture
> "What should I focus on next?", "Is this the right approach?",
> "Do you want me to continue or pivot?"

**synchronous_first** — asks about meeting scheduling, verbal alignment
> "Can we jump on a call about this?", "Did you sync with [person]?",
> "When can we meet to review this?"

### Step 3 — Generate answers

For each question, derive a crisp answer from the report data:

- **Answer format**: 1–3 sentences max. Direct. Factual.
- **Always cite the data**: reference actual commits, PRs, metrics from the report
- **Flag if data is missing**: "I don't have this in front of me — I'll follow up by [day]"
- **Include a "confidence signal"**: end answers to risk questions with a confidence statement
- Example: *"Coverage is at 87% on the auth module. No edge cases known to be uncovered."*

### Step 4 — Flag danger questions

Mark 2–4 questions as ⚠️ **DANGER** — questions where:
- The data in the report is weak or ambiguous
- The manager is likely to push back or dig deeper
- The user might not have a clean answer

For each danger question, provide a **deflection + follow-up strategy**:
> ⚠️ *"What's the test coverage on the billing module?"*
> Answer: "I don't have the exact number here — I'll pull that and send it to you within the hour."
> Strategy: Don't guess. Commit to a fast follow-up. It shows reliability, not weakness.

### Step 5 — Generate the HTML

Structure:

```html
<!-- CHEAT SHEET (always visible at top, never collapses) -->
<div class="cheat-sheet">
  <h2>30-second cheat sheet</h2>
  <ul>
    <li>✅ <strong>Top win:</strong> [biggest thing from the report]</li>
    <li>📊 <strong>Key number:</strong> [most impressive metric]</li>
    <li>⚡ <strong>Current blocker:</strong> [if any, else "None"]</li>
    <li>🎯 <strong>Your ask:</strong> [what you need from manager]</li>
    <li>⚠️ <strong>Danger zone:</strong> [topic most likely to get pushback]</li>
  </ul>
</div>

<!-- Q&A SECTION (collapsible per question) -->
<details class="qa-item">
  <summary>
    <span class="q-label">Q</span>
    [Question text]
  </summary>
  <div class="answer">
    <span class="a-label">A</span>
    [Answer text — 1-3 sentences, data-grounded]
  </div>
</details>

<!-- DANGER QUESTION (highlighted) -->
<details class="qa-item danger">
  <summary>
    <span class="q-label danger">⚠️ Q</span>
    [Danger question]
  </summary>
  <div class="answer">
    <strong>Answer:</strong> [prepared answer]<br/>
    <strong>Strategy:</strong> [how to handle pushback]
  </div>
</details>
```

**CSS** — dark theme, compact, readable on a phone before a meeting:

```css
body { font-family: -apple-system, sans-serif; background: #0f172a; color: #e2e8f0; max-width: 680px; margin: 0 auto; padding: 24px 16px; }
.cheat-sheet { background: #1e293b; border-left: 4px solid #2563eb; padding: 16px 20px; border-radius: 8px; margin-bottom: 24px; }
.cheat-sheet h2 { margin: 0 0 12px; font-size: 14px; text-transform: uppercase; letter-spacing: 0.5px; color: #94a3b8; }
.cheat-sheet li { margin: 6px 0; font-size: 15px; list-style: none; }
details { margin: 8px 0; border-radius: 6px; background: #1e293b; overflow: hidden; }
summary { padding: 14px 16px; cursor: pointer; font-size: 15px; font-weight: 500; display: flex; align-items: flex-start; gap: 10px; }
summary:hover { background: #263349; }
.q-label { background: #2563eb; color: white; font-size: 11px; font-weight: 700; padding: 2px 7px; border-radius: 4px; flex-shrink: 0; margin-top: 2px; }
.q-label.danger { background: #dc2626; }
details.danger summary { border-left: 3px solid #dc2626; }
.answer { padding: 12px 16px 16px 42px; font-size: 14px; line-height: 1.6; color: #cbd5e1; border-top: 1px solid #334155; }
```

Apply DESIGN.md tokens if present (accent color → `#2563eb` replacement, bg → `#0f172a` replacement).

### Step 6 — Save and confirm

Save to `./useless-report/[date]/[manager-name]-qa.html`.

```
Q&A prêt → useless-report/2026-05-01/alice-qa.html
15 questions · 3 zones de danger identifiées
Ouvre-le avant ta réunion : open useless-report/2026-05-01/alice-qa.html
```

## Rules

- **Answers come only from the report.** Never invent metrics or outcomes.
- **Questions come from the archetype.** Don't generate generic questions — use the manager's known patterns.
- **Danger zones are honest.** Don't mark everything as safe. If data is weak, flag it.
- **Mobile-first.** This will be read on a phone 5 minutes before a meeting.
- **No fabrication.** If there's no data to answer a question, the answer is "I'll follow up."

## Pipeline position

```
generate-X-report (markdown)
        ↓
  format-as-qa  ← you are here
        ↓
  [manager]-qa.html
        ↓
  Read on phone before meeting. Walk in ready.
```
