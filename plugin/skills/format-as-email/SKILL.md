---
name: format-as-email
description: >
  Converts a markdown manager report (the output of any useless-report generate-* skill) into an
  email-safe HTML file with fully inlined CSS — compatible with Gmail, Outlook, Apple Mail, and
  any webmail. No CSS variables, no external resources, no JavaScript. Colors and typography
  derived from DESIGN.md if present (tokens inlined as literal values, not CSS variables).
  Use this skill when the user wants to send the report directly by email, asks for "email-ready",
  "copy-paste into Gmail", "forward to my team", or needs a format that survives email client
  rendering. Triggers after any generate-* skill when the output destination is an email client.
---

# format-as-email

## Goal

Convert a markdown report into a single HTML file with all CSS **inlined as `style` attributes** — not in a `<style>` block, not as CSS variables. This is the only format that survives every email client, including Outlook's broken CSS engine.

> Your brand. Inline everywhere. Zero CSS-in-inbox surprises.

## Pourquoi inline et pas `<style>`

Gmail strips `<style>` blocks in forwarded emails. Outlook ignores most non-inline CSS. Apple Mail handles `<style>` but not reliably with CSS variables. The only safe approach: every styling property as a `style=""` attribute directly on the element.

## How to use

### Step 1 — Get the markdown

Same as `format-as-html`: the user provides a generated report.

### Step 2 — Load the visual identity

1. Look for `./DESIGN.md` in the current working directory.
2. **If found** → parse the YAML front matter, extract tokens as **literal values** (pas de CSS variables — les email clients ne les supportent pas).
3. **If not found** → invoquer `useless-report:generate-design`, puis continuer.

**Résolution des tokens en valeurs concrètes :**

| Propriété | DESIGN.md source | Fallback |
|---|---|---|
| fond général | `colors.neutral` | `#ffffff` |
| texte principal | `colors.primary` | `#1a1a1a` |
| accent / liens | `colors.secondary` → `colors.tertiary` | `#2563eb` |
| fond code/th | `colors.neutral` légèrement contrasté | `#f5f5f5` |
| séparateurs | `colors.primary` à 10% opacité | `#e5e5e5` |
| fonte corps | `typography.body-md.fontFamily` | `Arial, sans-serif` |
| fonte titres | `typography.h1.fontFamily` | `Georgia, serif` |
| taille corps | `typography.body-md.fontSize` | `16px` |
| interligne | `typography.body-md.lineHeight` | `1.6` |
| radius | `rounded.sm` | `4px` |

**Note sur les fontes email :** utiliser uniquement des fontes web-safe dans la stack (`Arial`, `Helvetica`, `Georgia`, `Times New Roman`, `Courier New`). Si le DESIGN.md spécifie une fonte custom (ex: `Inter`), la mettre en premier dans la stack avec une web-safe en fallback : `"Inter", Arial, sans-serif`.

**Couleurs sémantiques (ON TRACK / AT RISK / BLOCKED) :** toujours fixes, inlinées directement.

### Step 3 — Générer le HTML email

Structure à respecter :

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Titre du rapport]</title>
  <!--[if mso]>
  <noscript><xml><o:OfficeDocumentSettings><o:PixelsPerInch>96</o:PixelsPerInch></o:OfficeDocumentSettings></xml></noscript>
  <![endif]-->
</head>
<body style="margin:0; padding:0; background-color:[colors.neutral]; font-family:[typography.body-md.fontFamily], Arial, sans-serif;">
  <table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0"
         style="background-color:[colors.neutral];">
    <tr>
      <td align="center" style="padding:32px 16px;">
        <table role="presentation" width="600" cellspacing="0" cellpadding="0" border="0"
               style="max-width:600px; width:100%; background-color:[colors.neutral];">

          <!-- HEADER -->
          <tr>
            <td style="padding:0 0 24px 0; border-bottom:2px solid [rule-color]; color:[muted]; font-size:12px; text-transform:uppercase; letter-spacing:0.5px; font-family:[font-body], Arial, sans-serif;">
              [REPO] · [DATE] · généré par useless-report
            </td>
          </tr>

          <!-- TITLE (H1) -->
          <tr>
            <td style="padding:24px 0 8px 0;">
              <h1 style="margin:0; font-family:[font-heading], Georgia, serif; font-size:[h1.fontSize]; font-weight:[h1.fontWeight]; color:[colors.primary]; line-height:1.2;">
                [Titre]
              </h1>
            </td>
          </tr>

          <!-- STATUS BANNER (si présent) -->
          <!-- ON TRACK -->
          <tr>
            <td style="padding:8px 0;">
              <span style="display:inline-block; background:#dcfce7; color:#166534; padding:8px 16px; border-radius:[rounded.sm]; font-weight:600; font-size:14px; font-family:Arial, sans-serif;">
                ON TRACK
              </span>
            </td>
          </tr>

          <!-- CONTENU (une row par section H2) -->
          <tr>
            <td style="padding:24px 0 0 0;">
              <!-- H2 -->
              <h2 style="margin:0 0 8px 0; font-family:[font-heading], Georgia, serif; font-size:20px; font-weight:[h1.fontWeight]; color:[colors.primary]; padding-bottom:6px; border-bottom:1px solid [rule-color];">
                [Section]
              </h2>
              <!-- Paragraphes -->
              <p style="margin:0 0 12px 0; font-family:[font-body], Arial, sans-serif; font-size:[body.fontSize]; line-height:[body.lineHeight]; color:[colors.primary];">
                [Texte]
              </p>
              <!-- Listes -->
              <ul style="margin:0 0 12px 0; padding-left:20px; font-family:[font-body], Arial, sans-serif; font-size:[body.fontSize]; line-height:[body.lineHeight]; color:[colors.primary];">
                <li style="margin-bottom:4px;">[Item]</li>
              </ul>
              <!-- H3 -->
              <h3 style="margin:16px 0 6px 0; font-size:15px; font-weight:600; color:[colors.secondary]; font-family:[font-body], Arial, sans-serif;">
                [Sous-section]
              </h3>
              <!-- Code inline -->
              <code style="font-family:'Courier New', monospace; font-size:13px; background:[code-bg]; padding:1px 4px; border-radius:3px; color:[colors.primary];">
                [code]
              </code>
              <!-- Blockquote -->
              <blockquote style="margin:12px 0; padding:0 0 0 14px; border-left:4px solid [colors.secondary]; color:[muted]; font-style:italic; font-family:[font-body], Arial, sans-serif; font-size:[body.fontSize]; line-height:[body.lineHeight];">
                [Citation]
              </blockquote>
            </td>
          </tr>

          <!-- TABLEAU -->
          <tr>
            <td style="padding:12px 0;">
              <table width="100%" cellspacing="0" cellpadding="0" border="0"
                     style="border-collapse:collapse; font-family:[font-body], Arial, sans-serif; font-size:14px;">
                <tr>
                  <th style="text-align:left; padding:8px 10px; background:[code-bg]; border-bottom:1px solid [rule-color]; font-weight:600; color:[colors.primary];">
                    [En-tête]
                  </th>
                </tr>
                <tr>
                  <td style="padding:8px 10px; border-bottom:1px solid [rule-color]; color:[colors.primary];">
                    [Valeur]
                  </td>
                </tr>
              </table>
            </td>
          </tr>

          <!-- FOOTER -->
          <tr>
            <td style="padding:40px 0 0 0; border-top:1px solid [rule-color];">
              <p style="margin:0; font-size:11px; color:[muted]; font-family:Arial, sans-serif;">
                Généré par <a href="https://github.com/baconnier/useless-report" style="color:[colors.secondary]; text-decoration:none;">useless-report</a>
                · [timestamp]
              </p>
            </td>
          </tr>

        </table>
      </td>
    </tr>
  </table>
</body>
</html>
```

**Remplacer toutes les valeurs entre crochets** par les tokens résolus du DESIGN.md (valeurs littérales hex, px, etc. — jamais de `var(--...)` ni de références DESIGN.md dans le HTML final).

### Step 4 — Valeurs dérivées à calculer

- `[rule-color]` : prendre `colors.primary`, le convertir en hex, réduire l'opacité à 10% sur fond neutre. Exemple : primary `#1a1a1a` sur fond `#ffffff` → `#e5e5e5`.
- `[muted]` : primary à ~45% d'opacité sur fond neutre. Exemple : `#1a1a1a` → `#888888`.
- `[code-bg]` : neutral légèrement assombri. Exemple : `#ffffff` → `#f5f5f5`.
- Pour les couleurs DESIGN.md en rgba/hsl, les convertir en hex.

### Step 5 — Save and report

Save to `./useless-report-email-[YYYY-MM-DD].html`.

Confirmer :
```
Rapport email sauvegardé → ./useless-report-email-2026-05-01.html
Ouvre-le dans un navigateur, sélectionne tout (Cmd+A), copie (Cmd+C), colle dans Gmail / Outlook.
Ou envoie le fichier .html en pièce jointe si ton client le supporte.
```

## Règles

- **Zero CSS variables dans le HTML final.** Chaque `style=""` ne contient que des valeurs littérales.
- **Structure table-based** pour la mise en page (compatibilité Outlook/Exchange).
- **Pas de flexbox/grid.** Pas de `position: absolute`. Pas de `border-radius` complexe.
- **Max-width 600px.** Standard email.
- **Commentaire Outlook `<!--[if mso]>`** dans le `<head>` pour corriger le rendu Word engine.
- **Fontes web-safe en fallback** : toujours terminer la stack avec `Arial, sans-serif` ou `Georgia, serif`.
- **Pas de JavaScript.** Aucun `<script>`.
- **Fidèle au contenu source.** Format seulement, pas de réécriture.

## Pipeline position

```
DESIGN.md (ou generate-design si absent)
              ↓
generate-X-report (markdown output)
              ↓
       format-as-email  ← you are here
              ↓
   Gmail / Outlook / Apple Mail (copier-coller ou pièce jointe)
```
