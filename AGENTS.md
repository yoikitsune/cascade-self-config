# AGENTS.md — cascade-self-config

> Skill global d'auto-configuration de Cascade. Permet à Cascade d'analyser et d'améliorer son propre comportement en modifiant sa configuration `.devin/` (rules, skills, AGENTS.md) à partir du diagnostic de conversations passées.

## Quick Start

```bash
# Installer le skill globalement (Linux/macOS) — idempotent
./scripts/install-skills.sh

# Vérifier l'installation
./scripts/install-skills.sh --list

# Désinstaller
./scripts/install-skills.sh --remove
```

Sur Windows : `.\scripts\install-skills.ps1` (junctions, pas besoin d'admin).

## Tech Stack

- **Type** : Skill pur (pas de code, pas de CLI) — procédure markdown + références
- **Dépendance forte** : `dcr` (Devin Conversations Retriever) pour l'analyse de conversations en Phase 1
- **Distribution** : symlink global via `scripts/install-skills.{sh,ps1}` (per ADR-0001, adoptant ADR-0007 du projet dcr)

## Project Structure

```
.
├── AGENTS.md              # Vous êtes ici — point d'entrée pour les agents
├── docs/
│   ├── index.md           # Routeur de documentation
│   └── decisions/         # Architecture Decision Records (ADR)
│       └── 0001-adopt-adr-0007-symlink-distribution.md
├── progress.md            # Tableau de bord vivant
├── TODO.md                # Tâches différées (hors scope courant)
├── scripts/
│   ├── install-skills.sh   # Installation globale (Linux/macOS, symlinks)
│   └── install-skills.ps1  # Installation globale (Windows, junctions)
└── .devin/
    └── skills/
        └── cascade-self-config/
            ├── SKILL.md    # Procédure du skill (351 lignes)
            └── references/ # Guides génériques + patterns dcr + templates
                ├── agents-md-guide.md
                ├── dcr-diagnostic-patterns.md
                ├── diagnostic-catalog-template.md
                ├── project-tooling-template.md
                ├── rules-guide.md
                └── skills-guide.md
```

## Conventions

- Le skill vit dans `.devin/skills/cascade-self-config/` (source canonique versionnée)
- L'installation globale crée un symlink depuis `~/.codeium/windsurf/skills/cascade-self-config` vers le repo
- Les `references/` génériques (guides rules/skills/agents-md) sont partagées entre tous les projets
- Les `references/` projet-spécifiques (`diagnostic-catalog.md`, `project-tooling.md`) vivent dans `<projet>/.devin/skills/cascade-self-config/references/` — pas dans ce repo
- **Pas de global rule** — l'awareness est porté par la description tier-1 du skill (per ADR-0001)

## What NOT to Do

- Ne pas créer de `global_rules.md` — obsolète per ADR-0001 (l'awareness vient du skill)
- Ne pas copier le skill manuellement dans `~/.codeium/windsurf/skills/` — utiliser `scripts/install-skills.sh` (sinon drift)
- Ne pas committer de références projet-spécifiques dans ce repo — elles appartiennent aux projets utilisateurs
- Ne pas introduire de code applicatif — ce repo ne contient que des artifacts de configuration

## Current State

Voir `progress.md` pour le statut live et `docs/index.md` pour le routeur de documentation complet.
