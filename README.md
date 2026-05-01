<p align="center">
  <img src="assets/images/carousel.gif" width="100%" alt="useless-report"/>
</p>

<h1 align="center">useless-report</h1>

<p align="center">
  <strong>Your work is real. Make sure it reads that way.</strong>
</p>

<p align="center">
  One command. Your git history. Every manager, covered.
</p>

<p align="center">
  <a href="#get-started">Get started</a> · <a href="#how-it-works">How it works</a> · <a href="#the-10-archetypes">Archetypes</a> · <a href="#formats">Formats</a>
</p>

---

## The problem

You shipped. You fixed. You unblocked three people and made a call that saved the quarter.

Then someone asked for "a quick update" — and you spent two hours writing a report nobody read the same way.

**Same week. Ten different managers. Ten different expectations.**

---

## The fix

useless-report reads your codebase — commits, PRs, tickets — and writes the report *they* want to read.

Not a summary. Not a template. A report adapted to how your manager thinks, decides, and worries.

> *"Different style. Same truth."*

---

## One command

```
/useless-report:weekly-report
```

Set up once. Run forever. No questions after the first time.

```
✓ 23 commits · 5 PRs · 3 tickets ingested
✓ Brand identity loaded from DESIGN.md
→ alice-control.html       (HTML, branded)
→ alice-control-email.html (email-safe, inline CSS)
→ bob-risk-deck.md         (Marp slides → PDF)
```

---

## How it works

<table>
<tr>
<td width="25%" align="center">

**① Read**

Your git log.<br/>Your PRs.<br/>Your tickets.

*Real data, nothing invented.*

</td>
<td width="25%" align="center">

**② Understand**

Who is reading this?<br/>What do they need?<br/>How do they decide?

*10 manager archetypes.*

</td>
<td width="25%" align="center">

**③ Generate**

The report they'll actually read.<br/>Adapted tone, structure,<br/>and level of detail.

*One fact base. N reports.*

</td>
<td width="25%" align="center">

**④ Render**

HTML. Email. Slides.<br/>Your company's brand.<br/>One file. No dependencies.

*Ready to send.*

</td>
</tr>
</table>

---

## The 10 archetypes

Every manager has a pattern. Pick theirs — or let useless-report figure it out.

| | Archetype | What they always say |
|---|---|---|
| 🔬 | **Control-oriented** | *"Can you send a quick detailed breakdown?"* |
| 🛡️ | **Risk-sensitive** | *"Are we sure about this?"* |
| 📋 | **Process-heavy** | *"Is this in the tracking system?"* |
| 🎭 | **Stakeholder-oriented** | *"How does this look to the committee?"* |
| 🌀 | **Low-context** | *"Sorry catching up — what did we decide?"* |
| 🎯 | **Volatile-priority** | *"Actually, forget what I said last week"* |
| ⏰ | **Deadline-reactive** | *"We need this by tomorrow, right?"* |
| 🔍 | **Quality-maximalist** | *"Did we consider all the edge cases?"* |
| 🌊 | **Ambiguity-tolerant** | *"Yeah just run with it"* |
| 📞 | **Synchronous-first** | *"Let's jump on a quick call"* |

Don't know which fits? Paste three of their messages:

```
/useless-report:classify-manager-style
```

---

## Formats

Every report, in the format they'll actually open.

<table>
<tr>
<td align="center" width="25%">

**Markdown**

Slack. GitHub.<br/>Notion. Docs.

</td>
<td align="center" width="25%">

**HTML**

One file.<br/>Your brand.<br/>Email-ready.

</td>
<td align="center" width="25%">

**Email**

Inline CSS.<br/>Gmail. Outlook.<br/>Copy-paste.

</td>
<td align="center" width="25%">

**Slides**

Marp deck.<br/>PDF or PPTX.<br/>Steering committee.

</td>
</tr>
</table>

Visual identity driven by your `DESIGN.md` — the [Google Labs open standard](https://github.com/google-labs-code/design.md) for brand tokens. Pass a URL, a CSS file, a screenshot, or nothing — useless-report handles it.

---

## Get started

**Install** the plugin in Claude Code, then from inside any git repo:

```
/useless-report:weekly-report
```

First run takes 60 seconds to set up. Every run after: instant.

**Set up CI** (auto-comment on every PR):

```
/useless-report:setup-github-action
```

---

## The fine print

This is not a solution to bad management.

It is a tactical tool for reducing communication friction while the real problems get addressed. The profiles are not diagnoses. The archetypes have funny aliases because the situations are genuinely absurd — but the tool treats everyone with respect.

> *Work does not speak for itself. It has to be rendered for its audience.*

---

<p align="center">
  Part of the <a href="https://github.com/bacoco/useless-skills">useless-skills</a> family
</p>
