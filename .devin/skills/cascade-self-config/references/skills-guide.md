# Guide des Skills Cascade

## Structure

Un skill est un dossier contenant un fichier `SKILL.md` obligatoire et des fichiers de référence optionnels :

```
skill-name/
├── SKILL.md (obligatoire)
└── references/ (optionnel)
    ├── guide1.md
    └── guide2.md
```

## Syntaxe SKILL.md

Le fichier `SKILL.md` doit avoir un frontmatter YAML obligatoire avec exactement 2 champs :

```yaml
---
name: skill-name
description: Brief explanation shown to the model to help it decide when to invoke the skill
---
```

### champs obligatoires

- **name** : Identifiant unique du skill (affiché dans l'UI et utilisé pour les @-mentions)
- **description** : Brève explication montrée au modèle pour l'aider à décider quand invoquer le skill

### Règles de formatage critiques

- Frontmatter avec **exactement 3 tirets** `---` (pas 4)
- Le frontmatter doit être le **tout premier contenu** du fichier — aucun titre ni commentaire avant
- La description ne doit pas contenir `: ` (colon+espace) — cela casse le parsing YAML
- Vérifier après création : `head -1 SKILL.md` doit afficher `---`
- Contenu < 500 lignes

## Skill Scopes

### Skills projet
- `.windsurf/skills/` (emplacement documenté)
- `.devin/skills/` (emplacement utilisé par ce projet)
- `.agents/skills/`
- `.claude/skills/`

### Skills globaux (utilisateur)
- `~/.codeium/windsurf/skills/` (emplacement recommandé)
- `~/.agents/skills/`
- `~/.claude/skills/`

### Skills système (Enterprise)
- macOS : `/Library/Application Support/Windsurf/skills/`
- Linux : `/etc/windsurf/skills/`
- Windows : `C:\ProgramData\Windsurf\skills\`

## Invocation des skills

### Invocation automatique
Cascade décide d'invoquer un skill en fonction de sa `description`. Une description claire et spécifique est essentielle.

### Invocation manuelle
L'utilisateur peut invoquer un skill avec `@skill-name`.

## Progressive Disclosure

Les skills doivent suivre le principe de "progressive disclosure" :

1. **Description claire** dans le frontmatter pour que Cascade sache quand invoquer le skill
2. **Section "Quand utiliser ce skill"** explicite dans le contenu
3. **Procédure structurée** avec des étapes claires
4. **Références optionnelles** dans `references/` pour les détails complexes

## Exemple de structure

```markdown
---
name: deploy-to-production
description: Guides the deployment process to production with safety checks
---

## Pre-deployment Checklist
1. Run all tests
2. Check for uncommitted changes
3. Verify environment variables

## Deployment Steps
Follow these steps to deploy safely...

[Reference supporting files in this directory as needed]
```

## Best practices (documentation officielle)

1. **Descriptions claires** : La description aide Cascade à décider quand invoquer le skill. Être spécifique sur ce que le skill fait et quand l'utiliser.
2. **Inclure des ressources pertinentes** : Templates, checklists, et exemples rendent les skills plus utiles.
3. **Noms descriptifs** : `deploy-to-staging` est meilleur que `deploy1`. Les noms doivent indiquer clairement ce que le skill fait.

## Skills vs Rules vs Workflows

| Type | Format | Invocation | Usage |
|---|---|---|---|
| **Skill** | `SKILL.md` + références | `@mention` (manuel) ou automatique | Procédures complexes multi-étapes |
| **Rule** | `.md` avec frontmatter | `always_on`, `glob`, `model_decision`, `manual` | Contraintes comportementales courtes |
| **Workflow** | `.md` dans `workflows/` | `/slash-command` | Tâches répétitives avec étapes définies |

## Limites

- SKILL.md : < 500 lignes
- Éviter les skills trop génériques qui se chevauchent
- Mettre à jour `.devin/AGENTS.md` (inventory) lors de la création d'un nouveau skill
