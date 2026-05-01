---
name: format-as-slides
description: >
  Converts a markdown manager report (the output of any useless-report generate-* skill) into a
  Marp-compatible slide deck — markdown with frontmatter that can be rendered to PDF, PPTX, or
  HTML slides via marp-cli. Visual style is derived entirely from the project's DESIGN.md brand
  tokens — no hardcoded themes. If no DESIGN.md is found, runs the generate-design flow first.
  Use this skill when the user wants a presentation version of their report, asks for "slides",
  "a deck", "PowerPoint", "something for the meeting", or needs to present findings to a steering
  committee or all-hands. Triggers after any generate-* skill when the user mentions a
  presentation context.
---

# format-as-slides

## Goal

Convert a markdown report into a **Marp** slide deck with brand-accurate styling derived from `DESIGN.md`. The user gets a `.md` file they can convert to PDF, PPTX, or HTML.

> One markdown file. Three output formats. Your brand. No PowerPoint pain.

## Why Marp

Marp is markdown-driven slides. The user runs:

```bash
marp deck.md --pdf       # → deck.pdf
marp deck.md --pptx      # → deck.pptx
marp deck.md --html      # → deck.html
marp deck.md --watch     # live preview
```

If `marp-cli` isn't installed: `npm install -g @marp-team/marp-cli` or `brew install marp-cli`.

## How to use

### Step 1 — Get the markdown report

Same as `format-as-html`: the user provides a generated report.

### Step 2 — Load the visual identity

1. Look for `./DESIGN.md` in the current working directory.
2. **If found** → parse the YAML front matter, extract tokens.
3. **If not found** → invoke `useless-report:generate-design` (it will ask the user for sources or generate a generic design). Continue once `DESIGN.md` exists.

**Token extraction for Marp:**

| Style target | DESIGN.md source | Fallback |
|---|---|---|
| `section background` | `colors.neutral` | `#ffffff` |
| `section color` | `colors.primary` | `#1a1a1a` |
| `h1, h2 color` | `colors.secondary` → `colors.tertiary` | `#2563eb` |
| `section font-family` | `typography.body-md.fontFamily` | `-apple-system, sans-serif` |
| `h1 font-family` | `typography.h1.fontFamily` | `Georgia, serif` |
| `h1 font-weight` | `typography.h1.fontWeight` | `600` |
| `a color` | `colors.secondary` | `#2563eb` |
| `.status-* border-radius` | `rounded.sm` | `4px` |

**Couleurs sémantiques (ON TRACK / AT RISK / BLOCKED) :** toujours fixes, hors charte.

### Step 3 — Decide the deck layout

Split the report into slides:

- **One slide per H2 section** — typically 5–10 slides for a weekly report.
- **Title slide** first (from H1 of the report).
- **Long sections** (more than ~6 bullets or a large table) → split across 2 slides with continuation indicator.
- **Status banner** (ON TRACK / AT RISK / BLOCKED) → on the title slide.
- **Final slide**: "What I need from you" — the most important slide for any manager audience.

### Step 4 — Generate the Marp markdown

Use `uncover` as the Marp base theme (most neutral, easiest to override with custom CSS). Inject all brand styling via the `style:` block:

```md
---
marp: true
theme: uncover
paginate: true
size: 16:9
header: 'useless-report — [période]'
footer: '[repo] · généré le [date]'
style: |
  /* Generated from DESIGN.md — do not edit manually */
  section {
    font-family: [typography.body-md.fontFamily /* fallback: -apple-system, sans-serif */];
    background: [colors.neutral /* fallback: #ffffff */];
    color: [colors.primary /* fallback: #1a1a1a */];
    font-size: [typography.body-md.fontSize /* fallback: 20px (Marp default) */];
  }
  h1, h2 {
    font-family: [typography.h1.fontFamily /* fallback: Georgia, serif */];
    font-weight: [typography.h1.fontWeight /* fallback: 600 */];
    color: [colors.secondary ?? colors.tertiary ?? colors.primary /* fallback: #2563eb */];
  }
  h3 { color: [colors.primary /* fallback: #1a1a1a */]; font-size: 1em; }
  a { color: [colors.secondary /* fallback: #2563eb */]; }
  strong { color: [colors.primary /* fallback: #1a1a1a */]; }
  code {
    background: [colors.neutral légèrement contrasté /* fallback: #f5f5f5 */];
    border-radius: [rounded.sm /* fallback: 4px */];
    padding: 2px 6px;
    font-size: 0.85em;
  }
  table { width: 100%; border-collapse: collapse; font-size: 0.85em; }
  th { background: [colors.neutral légèrement contrasté]; }
  /* Lead slides (title, asks) */
  section.lead {
    background: [colors.primary /* fallback: #1a1a1a */];
    color: [colors.neutral /* fallback: #ffffff */];
  }
  section.lead h1, section.lead h2 {
    color: [colors.secondary ?? colors.neutral /* fallback: #ffffff */];
  }
  /* Semantic status colors — not branded */
  .status-on-track { background: #dcfce7; color: #166534; padding: 8px 16px; border-radius: [rounded.sm /* fallback: 4px */]; display: inline-block; }
  .status-at-risk   { background: #fef3c7; color: #92400e; padding: 8px 16px; border-radius: [rounded.sm]; display: inline-block; }
  .status-blocked   { background: #fee2e2; color: #991b1b; padding: 8px 16px; border-radius: [rounded.sm]; display: inline-block; }
---

<!-- _class: lead -->

# [Titre du rapport]

### [Période couverte]

<span class="status-on-track">ON TRACK</span>

[Auteur ou équipe]

---

# Résumé

[3 bullets max depuis la section executive summary]

---

# Ce qui est fait

[Top 5–7 items]

- ...
- ...

---

# Décisions prises

[Chaque décision : Décision → pourquoi]

---

# Risques & mitigations

| Risque | Probabilité | Mitigation |
|--------|------------|------------|
| ...    | moyen      | ...        |

---

<!-- _class: lead -->

# Ce dont j'ai besoin

[Demandes explicites — la slide dont ils se souviendront]

- ...
- ...

---

# Annexe

[Optionnel : traceabilité, liens, IDs de tickets]
```

### Step 5 — Save and instruct

Save to `./useless-report-deck-[YYYY-MM-DD].md`. Then tell the user:

```bash
# Prévisualisation live :
marp useless-report-deck-<date>.md --watch

# Export PDF (recommandé pour le partage) :
marp useless-report-deck-<date>.md --pdf

# Export PowerPoint :
marp useless-report-deck-<date>.md --pptx
```

## Slide content rules

- **Maximum 6 bullets per slide.** If a section has more, split or condense.
- **No paragraphs.** Extract 3–5 bullets from prose.
- **Tables OK** but keep them small (4 columns × 5 rows max).
- **Bold the key word in each bullet** so the audience scans fast.
- **One asks slide** at the end — never bury what you need.
- **Status banner on the title slide** if the source report had one.

## Rules

- **Faithful to source.** Don't add facts that weren't in the markdown report.
- **Compress, don't fabricate.** Paraphrase honestly when tightening.
- **Skip the appendix slide** if the source had no appendix material.
- **No emoji** unless the source used them.

## Pipeline position

```
DESIGN.md (ou generate-design si absent)
              ↓
generate-X-report (markdown output)
              ↓
       format-as-slides  ← you are here
              ↓
       marp → PDF / PPTX / HTML
```
