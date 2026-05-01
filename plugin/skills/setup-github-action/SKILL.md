---
name: setup-github-action
description: >
  Installe une GitHub Action dans le repo courant qui génère automatiquement un commentaire
  useless-report sur chaque Pull Request. À l'ouverture ou la mise à jour d'une PR, la CI lit
  les commits, détecte le profil manager configuré, génère le rapport adapté via l'API Claude,
  et le poste comme commentaire sur la PR. Use this skill when the user asks for "CI integration",
  "auto-comment on PRs", "GitHub Action for reports", or "automate the report on every PR".
---

# setup-github-action

## Goal

Générer et installer les fichiers nécessaires pour qu'useless-report tourne automatiquement en CI sur chaque PR : un workflow GitHub Actions + un script d'orchestration + les instructions de configuration des secrets.

> Chaque PR commente son propre résumé manager. Sans rien faire.

## Ce qui est installé

```
.github/
  workflows/
    useless-report-pr.yml      ← workflow GitHub Actions
  scripts/
    useless-report-ci.sh       ← script d'orchestration
.useless-report/
  config.yml                   ← config du projet (profil manager, format, etc.)
```

## How to use

### Step 1 — Collecter la config

Poser ces questions (toutes optionnelles — defaults indiqués) :

1. **Profil manager** : quel archétype ? (default: `control_oriented`). Peut être `auto` pour laisser Claude classifier à partir du nom du reviewer/assignee.
2. **Format du commentaire** : `markdown` (natif GitHub, default) ou `html` (collapsible block).
3. **Longueur** : `short` (3 bullets max, pour les PRs fréquentes), `standard` (rapport complet, default), `minimal` (titre + status + 1 ligne).
4. **Déclencheur** : `opened,synchronize` (default) — à chaque push sur la PR.
5. **Branch filter** : commenter uniquement sur les PRs vers `main`/`master` ? (default: toutes branches).

### Step 2 — Générer `.github/workflows/useless-report-pr.yml`

```yaml
name: useless-report PR comment

on:
  pull_request:
    types: [opened, synchronize, reopened]
    # branches: [main, master]   # décommenter pour filtrer

permissions:
  pull-requests: write
  contents: read

jobs:
  generate-report:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0    # nécessaire pour git log complet

      - name: Generate useless-report PR comment
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          PR_NUMBER: ${{ github.event.pull_request.number }}
          PR_TITLE: ${{ github.event.pull_request.title }}
          PR_BODY: ${{ github.event.pull_request.body }}
          BASE_SHA: ${{ github.event.pull_request.base.sha }}
          HEAD_SHA: ${{ github.event.pull_request.head.sha }}
          REPO: ${{ github.repository }}
        run: bash .github/scripts/useless-report-ci.sh

      - name: Post comment on PR
        if: always()
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            if (!fs.existsSync('/tmp/useless-report-comment.md')) {
              console.log('No report generated, skipping comment.');
              return;
            }
            const body = fs.readFileSync('/tmp/useless-report-comment.md', 'utf8');
            // Supprimer l'ancien commentaire useless-report s'il existe
            const comments = await github.rest.issues.listComments({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
            });
            for (const comment of comments.data) {
              if (comment.body.includes('<!-- useless-report -->')) {
                await github.rest.issues.deleteComment({
                  owner: context.repo.owner,
                  repo: context.repo.repo,
                  comment_id: comment.id,
                });
              }
            }
            // Poster le nouveau commentaire
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body,
            });
```

### Step 3 — Générer `.github/scripts/useless-report-ci.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail

# ── Config ──────────────────────────────────────────────────────────────
CONFIG_FILE=".useless-report/config.yml"
MANAGER_PROFILE="${MANAGER_PROFILE:-control_oriented}"
REPORT_LENGTH="${REPORT_LENGTH:-standard}"
OUTPUT_FILE="/tmp/useless-report-comment.md"

# Lire la config projet si elle existe
if [[ -f "$CONFIG_FILE" ]]; then
  profile=$(grep 'manager_profile:' "$CONFIG_FILE" | awk '{print $2}' | tr -d '"')
  [[ -n "$profile" ]] && MANAGER_PROFILE="$profile"
  length=$(grep 'comment_length:' "$CONFIG_FILE" | awk '{print $2}' | tr -d '"')
  [[ -n "$length" ]] && REPORT_LENGTH="$length"
fi

# ── Collecter les données ────────────────────────────────────────────────
# Commits de la PR
COMMITS=$(git log "${BASE_SHA}..${HEAD_SHA}" \
  --pretty=format:"- %s (%h) [%an]" \
  --no-merges 2>/dev/null || echo "(impossible de lire les commits)")

# Fichiers modifiés
FILES_CHANGED=$(git diff --name-only "${BASE_SHA}..${HEAD_SHA}" 2>/dev/null \
  | head -30 | sed 's/^/  - /' || echo "  (indisponible)")

# Stats
STATS=$(git diff --stat "${BASE_SHA}..${HEAD_SHA}" 2>/dev/null | tail -1 || echo "")

# ── Prompt Claude ────────────────────────────────────────────────────────
PROMPT="Tu es useless-report, un générateur de rapports d'avancement adaptés au profil manager.

## Contexte de la PR
- Repo : ${REPO}
- PR #${PR_NUMBER} : ${PR_TITLE}
- Description : ${PR_BODY:-'(aucune description)'}
- Stats : ${STATS}

## Commits inclus
${COMMITS}

## Fichiers modifiés
${FILES_CHANGED}

## Profil manager : ${MANAGER_PROFILE}
## Longueur demandée : ${REPORT_LENGTH}

Génère un commentaire de PR adapté au profil '${MANAGER_PROFILE}'.
- Si 'short' : 3 bullets max, pas de sections, direct.
- Si 'standard' : sections Résumé / Ce qui change / Risques / Ce que je demande.
- Si 'minimal' : une ligne de statut + une phrase.

Commence le commentaire par la ligne exacte : <!-- useless-report -->
Termine par une ligne : *Généré par [useless-report](https://github.com/baconnier/useless-report)*

Réponds uniquement avec le contenu markdown du commentaire, sans aucun texte d'introduction."

# ── Appel API Claude ─────────────────────────────────────────────────────
RESPONSE=$(curl -s https://api.anthropic.com/v1/messages \
  -H "x-api-key: ${ANTHROPIC_API_KEY}" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d "{
    \"model\": \"claude-sonnet-4-6\",
    \"max_tokens\": 1024,
    \"messages\": [{
      \"role\": \"user\",
      \"content\": $(echo "$PROMPT" | jq -Rs .)
    }]
  }")

# Extraire le texte de la réponse
COMMENT=$(echo "$RESPONSE" | jq -r '.content[0].text // empty')

if [[ -z "$COMMENT" ]]; then
  echo "Erreur API Claude : $(echo "$RESPONSE" | jq -r '.error.message // "réponse vide"')" >&2
  exit 1
fi

echo "$COMMENT" > "$OUTPUT_FILE"
echo "Rapport généré → $OUTPUT_FILE"
```

### Step 4 — Générer `.useless-report/config.yml`

```yaml
# useless-report project configuration
# Modifie ces valeurs pour personnaliser les rapports CI

manager_profile: control_oriented
# Profils disponibles : control_oriented, risk_sensitive, process_heavy,
# stakeholder_oriented, low_context, volatile_priority, deadline_reactive,
# quality_maximalist, ambiguity_tolerant, synchronous_first

comment_length: standard
# Valeurs : short | standard | minimal
```

### Step 5 — Instructions secrets GitHub

Afficher ces instructions à l'utilisateur :

```
Configuration requise dans GitHub :

1. Aller sur : https://github.com/[owner]/[repo]/settings/secrets/actions
2. Ajouter le secret : ANTHROPIC_API_KEY = [ta clé API Anthropic]

Le secret GITHUB_TOKEN est automatique — pas d'action requise.

Pour tester localement avant de pusher :
  export ANTHROPIC_API_KEY=sk-ant-...
  export BASE_SHA=$(git merge-base HEAD main)
  export HEAD_SHA=$(git rev-parse HEAD)
  export PR_NUMBER=0 PR_TITLE="Test local" PR_BODY="" REPO="owner/repo"
  bash .github/scripts/useless-report-ci.sh
  cat /tmp/useless-report-comment.md
```

### Step 6 — Confirmer l'installation

```
GitHub Action installée :
  .github/workflows/useless-report-pr.yml
  .github/scripts/useless-report-ci.sh
  .useless-report/config.yml

Prochaine étape : ajouter ANTHROPIC_API_KEY dans les secrets GitHub.
Ensuite, chaque PR recevra automatiquement un résumé adapté au profil '[manager_profile]'.
```

## Rules

- **Ne jamais committer les fichiers** — générer uniquement, laisser l'utilisateur committer.
- **Rendre les fichiers exécutables** : `chmod +x .github/scripts/useless-report-ci.sh`
- **Respecter la structure `<!-- useless-report -->`** dans le commentaire pour permettre la mise à jour (le script supprime l'ancien commentaire avant d'en créer un nouveau).
- **Utiliser `claude-sonnet-4-6`** dans le script CI — modèle actuel et capable. Mettre à jour si une version plus récente est disponible.
- **Ne pas stocker la clé API** dans les fichiers générés — toujours via `secrets.ANTHROPIC_API_KEY`.

## Pipeline position

```
Push sur une PR
      ↓
GitHub Actions trigger
      ↓
useless-report-ci.sh (git log + API Claude)
      ↓
Commentaire PR adapté au profil manager
      ↓
Mise à jour automatique à chaque push
```
