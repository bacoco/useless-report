---
name: generate-design
description: >
  Génère ou valide un fichier DESIGN.md (standard Google Labs / Stitch, avril 2026) à partir de
  la charte visuelle de l'entreprise. Accepte n'importe quelle combinaison d'entrées : URL du
  site corporate, fichiers CSS/SCSS/tokens.json, screenshot, description textuelle — ou rien du
  tout, auquel cas un design générique professionnel est généré. Utilisé automatiquement par
  format-as-html et format-as-slides quand aucun DESIGN.md n'est présent. Déclencher directement
  avec /useless-report:generate-design pour initialiser la charte d'un nouveau projet.
---

# generate-design

## Goal

Produire un `DESIGN.md` valide (spec [google-labs-code/design.md](https://github.com/google-labs-code/design.md) v0.1.0) à partir de ce que l'utilisateur peut fournir. Le fichier devient la source de vérité visuelle pour tous les formats générés par useless-report.

> Pas de DESIGN.md → on en crée un. Avec ce que tu as, ou sans rien.

## Quand ce skill s'exécute

- **Invoqué directement** : `/useless-report:generate-design`
- **Invoqué automatiquement** : par `format-as-html` ou `format-as-slides` si `./DESIGN.md` est absent du répertoire courant

## Flow

### Étape 1 — Vérifier si DESIGN.md existe

Chercher `./DESIGN.md` dans le répertoire courant.

**Si présent :**
- Valider la structure : sections requises présentes (Overview, Colors, Typography), références de tokens non brisées (`{colors.x}` pointe vers un token existant)
- Si valide → retourner les tokens parsés au skill appelant
- Si invalide → signaler les erreurs et proposer de corriger ou de regénérer

**Si absent → passer à l'étape 2.**

### Étape 2 — Demander les sources disponibles

```
Pour générer ta charte visuelle, donne-moi ce que tu as :
  - URL du site (ex: https://monentreprise.com)
  - Fichier CSS / SCSS / tokens.json / tailwind.config.js
  - Screenshot ou logo (image)
  - Description en texte libre ("bleu marine, typo sans-serif moderne, style corporate")
  - Ou appuie sur Entrée pour un design générique professionnel
```

Si appelé automatiquement (depuis format-as-html ou format-as-slides), poser la question sans bloquer le pipeline — si l'utilisateur ne répond pas ou passe, continuer avec le design générique.

### Étape 3 — Extraire les tokens selon les sources

Appliquer dans l'ordre de priorité les sources fournies :

**URL** → scraper la page :
- Extraire les `font-family` déclarées dans le CSS
- Extraire les couleurs dominantes (`color`, `background-color`, `--color-*`, `--brand-*` CSS variables)
- Déduire primary (couleur de texte principale), secondary/accent (CTA, liens), neutral (fond)

**CSS / SCSS / tokens.json / tailwind.config.js** → parser :
- Variables CSS : `--color-primary`, `--color-brand`, `--font-*`, `--radius-*`, `--spacing-*`
- Design tokens W3C DTCG : `{ "$value": "...", "$type": "color" }`
- Tailwind config : `theme.colors`, `theme.fontFamily`, `theme.borderRadius`, `theme.spacing`

**Screenshot / image** → analyse visuelle multimodale :
- Extraire la palette de couleurs dominante (primary, secondary, neutral)
- Identifier la famille typographique (serif / sans-serif / monospace, style général)
- Estimer les rayons de coins (sharp / medium / rounded)

**Description textuelle** → interprétation sémantique :
- "bleu marine" → `#1B2A4A`, "bleu corporate" → `#003366`, "bleu vif" → `#0066CC`
- "sans-serif moderne" → `Inter, system-ui`, "serif élégant" → `Georgia, "Times New Roman"`, "technique" → `"SF Mono", Consolas`
- "minimaliste" → espacement généreux, palette réduite
- "corporate" → typo neutre, couleurs sobres

**Rien fourni** → design générique professionnel :
```
primary:   #1a1a1a
secondary: #2563eb
neutral:   #ffffff
h1:        Georgia, serif — 28px/600
body:      system-ui, sans-serif — 16px/400/1.6
rounded:   sm:4px md:8px
spacing:   sm:8px md:16px
```
Afficher : *"Aucune charte détectée — design générique appliqué. Édite `DESIGN.md` pour personnaliser."*

Si plusieurs sources sont fournies, les fusionner : le fichier CSS/tokens prend la priorité sur l'URL, l'URL sur le screenshot, le screenshot sur la description.

### Étape 4 — Générer le DESIGN.md

Produire un fichier respectant la spec officielle :

```markdown
---
version: alpha
name: "[Nom extrait ou 'Generic']"
colors:
  primary: "[valeur hex]"
  secondary: "[valeur hex]"
  neutral: "[valeur hex]"
  # tertiary, on-primary, etc. si disponibles
typography:
  h1:
    fontFamily: "[valeur]"
    fontSize: "[valeur]px"
    fontWeight: [valeur]
    lineHeight: [valeur]
    letterSpacing: "[valeur]em"   # si disponible
  body-md:
    fontFamily: "[valeur]"
    fontSize: "[valeur]px"
    fontWeight: [valeur]
    lineHeight: [valeur]
rounded:
  sm: "[valeur]px"
  md: "[valeur]px"
spacing:
  sm: "[valeur]px"
  md: "[valeur]px"
---

## Overview

[2–3 phrases décrivant la marque, son ton, son audience cible. Si inféré, le préciser.]

## Colors

- **primary** (`[hex]`) — [rôle : texte principal, titres]
- **secondary** (`[hex]`) — [rôle : accent, liens, CTA]
- **neutral** (`[hex]`) — [rôle : fond, surfaces]

## Typography

- **Titres** : [fontFamily], [fontSize], weight [fontWeight]
- **Corps** : [fontFamily], [fontSize], weight [fontWeight], line-height [lineHeight]

## Layout

- Espacement de base : [spacing.sm] / [spacing.md]
- Coins : [rounded.sm] (petits éléments) / [rounded.md] (cards, modales)
```

Sauvegarder à `./DESIGN.md`.

### Étape 5 — Confirmer

```
DESIGN.md généré → ./DESIGN.md
Charte : [primary] / [secondary] / [neutral] · [typography.h1.fontFamily]
Édite ce fichier à tout moment pour affiner la charte.
```

Si invoqué automatiquement, ne pas afficher de confirmation verbeuse — juste continuer le pipeline.

## Règles

- **Jamais de valeurs inventées sans mention.** Si une valeur est inférée (pas extraite directement), commenter dans le YAML : `# inferred`
- **Ne pas écraser un DESIGN.md valide existant** sans demander confirmation explicite à l'utilisateur.
- **Respecter la spec google-labs-code/design.md v0.1.0** : structure YAML + corps Markdown, références de tokens avec syntaxe `{path.to.token}`.
- **Tokens de référence** : utiliser la syntaxe de référence quand c'est pertinent, ex: `button-primary.backgroundColor: "{colors.secondary}"`

## Position dans le pipeline

```
URL / CSS / screenshot / texte / rien
              ↓
       generate-design  ← vous êtes ici
              ↓
         DESIGN.md
              ↓
   format-as-html / format-as-slides
```
