---
name: weekly-report
description: >
  One-shot orchestrator that produces a complete manager-adapted weekly report from the current
  git repository. Runs the full useless-report pipeline: ingests git activity, optionally pulls
  GitHub PR data, classifies the manager (or uses a known profile), generates the adapted report,
  and optionally renders it as HTML or slides. Use this skill when the user asks "generate my
  weekly report", "what should I tell my manager about this week", "give me a status update for
  my boss", or any one-shot report request from inside a git repo. Triggers on Friday afternoon
  reports, sprint summaries, or whenever the user wants the entire pipeline executed without
  managing each step manually.
---

# weekly-report

## Goal

Run the entire useless-report pipeline end-to-end with minimal user input. The user is in their git repo, says "generate my weekly report for my [manager type] manager", and gets a finished artifact in markdown, HTML, or slides.

> One command. Real data. Adapted output. No copy-paste.

## How to use

### Step 1 — Gather inputs from the user

Ask only what's missing. Try to infer from defaults:

- **Period**: default last 7 days. If user says "this sprint" or "last 2 weeks", use that.
- **Manager profile**: ask the user. Options:
  - They already know: e.g., "control-oriented", "risk-sensitive", or any of the 10 archetypes
  - They don't know: offer to run `useless-report:classify-manager-style` first if they paste manager messages
  - Default if they refuse to specify: `control_oriented` (most common, safest assumption)
- **Output format**: ask. Options: `markdown` (default), `html`, `email`, `slides`, or `all` (generate all four).

### Step 2 — Run the ingestion


Invoke (or inline the logic of):
1. `useless-report:ingest-from-git` — always, this is the foundation
2. `useless-report:ingest-from-github` — if the repo has a GitHub remote and `gh` is authenticated
3. `useless-report:ingest-from-tickets` — if commits contain ticket IDs

Combine outputs into a single fact base.

### Step 3 — Charger la charte visuelle

Avant de formatter quoi que ce soit, vérifier la présence de `./DESIGN.md` :

- **Présent** → les étapes format-as-html / format-as-slides l'utiliseront automatiquement.
- **Absent** → proposer : *"Veux-tu que je génère un `DESIGN.md` pour appliquer la charte de ta boîte au rapport ? (URL, CSS, screenshot, ou Entrée pour un design générique)"*
  - Si l'utilisateur accepte → invoquer `useless-report:generate-design`
  - Si l'utilisateur refuse → continuer avec le design générique (les formats le feront automatiquement)

### Step 4 — Generate the adapted report

Based on the manager profile, invoke the matching `generate-*` skill with the fact base as input:

| Profile | Skill |
|---|---|
| `control_oriented` | `useless-report:generate-control-report` |
| `risk_sensitive` | `useless-report:generate-risk-report` |
| `process_heavy` | `useless-report:generate-process-report` |
| `stakeholder_oriented` | `useless-report:generate-stakeholder-brief` |
| `low_context` | `useless-report:generate-low-context-digest` |
| `volatile_priority` | `useless-report:generate-volatile-priority-update` |
| `deadline_reactive` | `useless-report:generate-deadline-reactive-update` |
| `quality_maximalist` | `useless-report:generate-quality-maximalist-report` |
| `ambiguity_tolerant` | `useless-report:generate-ambiguity-tolerant-update` |
| `synchronous_first` | `useless-report:generate-synchronous-first-update` |

The output is markdown.

### Step 5 — Render to requested format(s)

- If user asked for `markdown`: save to `./useless-report-[date].md` and you're done.
- If user asked for `html`: invoke `useless-report:format-as-html` on the markdown.
- If user asked for `email`: invoke `useless-report:format-as-email` — CSS inline, compatible Gmail/Outlook.
- If user asked for `slides`: invoke `useless-report:format-as-slides`.
- If user asked for `all`: produce all four (markdown, html, email, slides) and list the file paths.

### Step 6 — Final summary to the user

Tell the user:
- Which manager profile was used
- What was ingested (commits / PRs / tickets, with counts)
- Where the file(s) were saved
- One next-step suggestion (e.g., "open with `open [file]`" or "send to your manager")

## Example flow

User: *"Generate my weekly report — my manager is risk-sensitive, give me HTML."*

You:
1. Run `git log` to gather last 7 days of activity → 18 commits, 3 contributors
2. Run `gh pr list` → 4 PRs merged, 2 open
3. Extract tickets: `AUTH-184`, `BILL-77`
4. Check for `./DESIGN.md` → found (Stripe blue palette). Will be used automatically.
5. Invoke `generate-risk-report` with the fact base
6. Invoke `format-as-html` on the markdown → DESIGN.md tokens applied to CSS
7. Save to `./useless-report-2026-05-01.html`
8. Reply: *"Generated risk-focused report from 18 commits and 4 merged PRs. Charte Stripe appliquée depuis DESIGN.md. Saved to useless-report-2026-05-01.html. Open with `open useless-report-2026-05-01.html` or attach to your email."*

## Rules

- **Don't fabricate.** All facts come from git/GitHub. If something is missing, say so.
- **Be efficient.** Don't ask 5 clarifying questions. Default sensibly and tell the user what you defaulted to.
- **One file per format.** Don't scatter outputs across the filesystem.
- **Confirm the manager profile** in the final summary so the user can spot a wrong assumption.
- **Skip ingestion sources gracefully**: if `gh` isn't authenticated, just use git data and note it in the output.

## Tip for power users

You can pre-save your manager's profile in a file (e.g., `~/.useless-report/manager.yaml`) and skip the question. This skill respects that file if present.

```yaml
# ~/.useless-report/manager.yaml
profile: control_oriented
notes: "Likes options + traceability. Avoid surprise decisions."
```
