# useless-report

### the upward-compiler

*Because "quick update?" never means quick.*

---

Your commits say what changed.
Your tickets say what moved.
**useless-report** says it in the language your manager understands.

---

## What is this?

Most reporting tools answer: **What happened?**

useless-report answers: **How should this be communicated to *this specific person*?**

It reads your real engineering activity — git history, PRs, tickets — and turns it into a manager-adapted report. Same work, different format, depending on who's reading it. Output as markdown, polished HTML, or slides.

| Existing tools | useless-report |
|---|---|
| Generate status reports | Generates *audience-adapted* reports |
| Summarize Git / Jira activity | Reframes work for the reader's decision style |
| One generic output | Multiple psychological renderings |
| Markdown only | Markdown, HTML, or slides |
| Manual copy-paste | Reads your repo directly |

---

## Two core concepts

### Psychological rendering

Same data. Different report. Adapted to the reader.

Like frontend rendering: same data model, different components, different output depending on who's looking. Here: same engineering work, different format, adapted to the manager's decision style, anxiety level, and desired granularity.

### Controlled verbosity

> Never fabricate. Expand only from facts.

The style is adapted. The reality is not. Every sentence in the report must trace back to something real (a commit, a PR, a ticket, an explicit user statement). Verbosity is justified only when it serves the reader's need for context, control, traceability, or reassurance — not to fill space.

---

## Quick start (v2)

From inside any git repo:

```
/useless-report:weekly-report
```

The orchestrator will:

1. Read your git activity (last 7 days by default)
2. Optionally pull GitHub PRs and ticket references
3. Ask which manager archetype fits (or use a known profile)
4. Generate the adapted report
5. Render it as markdown, HTML, or slides

Done. One command, real data, no copy-paste.

---

## The pipeline

```
   ┌─────────────────────┐
   │  ingest-from-git    │   ← reads git log of current repo
   │  ingest-from-github │   ← gh CLI: PRs, issues, reviews
   │  ingest-from-tickets│   ← extracts JIRA/Linear/GH refs
   └──────────┬──────────┘
              │
              ▼
   ┌─────────────────────────┐
   │ classify-manager-style  │   ← optional: paste manager
   └──────────┬──────────────┘     messages, get a profile
              │
              ▼
   ┌──────────────────────────┐
   │ generate-[archetype]-*   │   ← 10 archetypes available
   └──────────┬───────────────┘
              │
              ▼
   ┌────────────────────┐
   │ format-as-html     │   ← polished, email-ready
   │ format-as-slides   │   ← Marp deck → PDF / PPTX
   │ (default markdown) │   ← Slack / GitHub / docs
   └────────────────────┘
```

You can run the pipeline end-to-end via `weekly-report`, or invoke any individual skill manually.

---

## The 10 manager archetypes

| Skill | Archetype | Funny alias |
|---|---|---|
| `generate-control-report` | Control-oriented | "Can you send me a quick detailed breakdown?" |
| `generate-risk-report` | Risk-sensitive | "Are we sure about this?" |
| `generate-process-report` | Process-heavy | "Is this in the tracking system?" |
| `generate-stakeholder-brief` | Stakeholder-oriented | "How does this look to the steering committee?" |
| `generate-low-context-digest` | Low-context | "Sorry, catching up — what did we decide?" |
| `generate-volatile-priority-update` | Volatile-priority | "Actually, forget what I said last week" |
| `generate-deadline-reactive-update` | Deadline-reactive | "We need this by tomorrow, right?" |
| `generate-quality-maximalist-report` | Quality-maximalist | "Did we consider all the edge cases?" |
| `generate-ambiguity-tolerant-update` | Ambiguity-tolerant | "Yeah just run with it" |
| `generate-synchronous-first-update` | Synchronous-first | "Let's jump on a call about this" |

Don't know which fits? Run `classify-manager-style` — paste a few of your manager's messages and get a profile in seconds.

---

## All skills

### Ingestion (read your repo)

```
useless-report:ingest-from-git
useless-report:ingest-from-github
useless-report:ingest-from-tickets
```

### Classification

```
useless-report:classify-manager-style
```

### Generation (10 archetype-specific reports)

```
useless-report:generate-control-report
useless-report:generate-risk-report
useless-report:generate-process-report
useless-report:generate-stakeholder-brief
useless-report:generate-low-context-digest
useless-report:generate-volatile-priority-update
useless-report:generate-deadline-reactive-update
useless-report:generate-quality-maximalist-report
useless-report:generate-ambiguity-tolerant-update
useless-report:generate-synchronous-first-update
```

### Formatting

```
useless-report:format-as-html
useless-report:format-as-slides
```

### Orchestration

```
useless-report:weekly-report      ← end-to-end pipeline in one shot
```

---

## Output formats

| Format | When to use | Output file |
|---|---|---|
| **Markdown** (default) | Slack, GitHub comment, internal docs, email body | `useless-report-[date].md` |
| **HTML** (via `format-as-html`) | Polished email, share link, printable artifact | `useless-report-[date].html` |
| **Slides** (via `format-as-slides`) | Steering committee, all-hands, exec presentations | `useless-report-deck-[date].md` (Marp) → render to PDF/PPTX |

All HTML output is **single-file, no external dependencies, no JavaScript** — works in any email client.
Slides require [Marp CLI](https://github.com/marp-team/marp-cli) to render to PDF/PPTX.

---

## Design principle

This project does not claim employees should compensate for bad management forever. It helps reduce communication friction when direct structural change is not immediately available.

> This is not a solution to bad management. It is a tactical tool for reducing noise while the real problems get addressed.

The profiles are not diagnoses. They are communication preference maps. The archetypes have funny aliases because the situations are genuinely absurd — but the tool treats people with respect.

---

## Part of the useless-skills family

→ [useless-roulette](https://github.com/bacoco/useless-skills) — absurd skills for Claude Code

---

*Work does not speak for itself. It has to be rendered for its audience.*
