---
name: generate-dev-change-report
description: >
  Generates a developer-facing change report from a repository's current changes, commits, PR
  context, saved screenshots, and before/after visual evidence. Use when the user asks for a dev
  report, change report, report on modifications, before/after screenshot report, UI change report,
  PR summary with visuals, "rapport de dev", "rapport sur les modifications", or wants to send a
  report about what changed after development.
---

# generate-dev-change-report

## Goal

Create a factual development report from real repo evidence: git changes, commits, tests, screenshots, and before/after visual artifacts. This is not a weekly manager report; it is a delivery report for one branch, PR, feature, fix, or visual change.

> Same truth as the code review, rendered for the person who needs to understand what changed.

## ShipGuard Boundary

This skill does not capture screenshots or drive a browser. ShipGuard owns visual evidence capture, before/after comparison, screenshot annotation, and durable visual artifacts.

Use this split:

- ShipGuard: capture screenshots, generate `change-reports` / `persona-reports`, keep before/after visual evidence.
- useless-report: read git/PR facts plus existing visual artifacts, then write a clear dev report that can be formatted as Markdown, HTML, email, slides, Q&A, or call prep.

If the user asks for a report with screenshots and no screenshots or ShipGuard artifacts exist, tell them to install/run ShipGuard first, or ask for the screenshot paths. Do not attempt to recreate ShipGuard inside useless-report.

## Inputs

Use whatever evidence is available:

- current git branch, status, diff, and recent commits
- GitHub PR metadata if a PR exists and `gh` is authenticated
- user-provided screenshot paths
- common local screenshot/report folders:
  - `visual-tests/_results/change-reports/`
  - `visual-tests/_results/persona-reports/`
  - `visual-tests/_results/screenshots/`
  - `screenshots/`
  - `assets/screenshots/`
  - `assets/images/`
- validation output the user pasted or that can be safely run

Do not invent missing evidence. If a before screenshot is missing, say so.

## Workflow

### Step 1 - Collect repo facts

Run lightweight git commands from the target repo:

```bash
git status -sb
git branch --show-current
git log --oneline --max-count=12
git diff --stat
git diff --name-only
```

If a base branch is known, prefer scoped commands:

```bash
git diff --stat <base>...HEAD
git diff --name-only <base>...HEAD
git log --oneline <base>..HEAD
```

If GitHub is available:

```bash
gh pr view --json title,url,baseRefName,headRefName,commits,files,reviews,statusCheckRollup
```

### Step 2 - Collect visual evidence

Use screenshot paths provided by the user first. Then inspect common folders.

Pair before/after images by filename patterns:

- `before-*.png` with `after-*.png`
- `*-before.png` with `*-after.png`
- `desktop-*-before.png` with `desktop-*-after.png`
- `mobile-*-before.png` with `mobile-*-after.png`

If a ShipGuard report exists, reference it:

```text
visual-tests/_results/change-reports/<context>/index.html
visual-tests/_results/change-reports/<context>/report.json
visual-tests/_results/persona-reports/<context>/index.html
```

If no visual evidence exists and the user asked for before/after screenshots, respond with the exact missing dependency:

```text
Before/after screenshots are not available. Run ShipGuard first, for example:
/sg-visual-run <scope>
/sg-change-report <context>
Then rerun useless-report:generate-dev-change-report.
```

When image files are local, embed them in Markdown with relative paths:

```markdown
![Before](path/to/before.png)
![After](path/to/after.png)
```

### Step 3 - Write the report

Default output:

```text
useless-report/<YYYY-MM-DD>/dev-change-report.md
```

Create the directory if needed. Use a more specific filename when the branch or feature name is clear:

```text
useless-report/<YYYY-MM-DD>/<branch-or-feature>-dev-change-report.md
```

Use this structure:

```markdown
# Dev Change Report - <feature or branch>

## Executive Summary

Short factual summary of what changed and why.

## Scope

- Branch:
- Base:
- PR:
- Files changed:
- Commits:

## What Changed

Group changes by product area or technical layer.

## Visual Evidence

Before/after pairs, screenshots, and links to ShipGuard reports.

## Validation

Commands run, status checks, manual checks, and visual checks.

## Risks And Limits

Known gaps, missing screenshots, unavailable before states, flaky checks, or follow-up needed.

## Reviewer Notes

What to inspect first and any decision needed.
```

## Optional formats

If the user wants a shareable artifact:

- HTML: invoke `useless-report:format-as-html`
- Email: invoke `useless-report:format-as-email`
- Slides: invoke `useless-report:format-as-slides`
- Q&A: invoke `useless-report:format-as-qa`
- Call prep: invoke `useless-report:format-as-call-prep`

Do not send anything automatically unless the user explicitly asks and the required mail or GitHub tooling is configured.

## Rules

- Ground every claim in git, PR metadata, screenshots, or provided validation.
- Preserve uncertainty: say "not verified" or "before screenshot unavailable" when true.
- Do not include secrets, tokens, private credentials, or raw `.env` contents.
- Keep the report useful for review: concise summary first, evidence and paths after.
- Prefer links to existing ShipGuard artifacts over duplicating long generated HTML in the Markdown report.
- Treat ShipGuard as the optional visual provider. The skill still works without ShipGuard for text-only dev reports, but visual before/after reports require screenshots from ShipGuard or user-provided paths.
