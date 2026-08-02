# Documentation Index — cascade-self-config

> Routeur source-of-truth. Dernière mise à jour : voir `progress.md`.

## Documentation principale

| Document | Rôle | Quand le lire |
|---|---|---|
| [`/AGENTS.md`](../AGENTS.md) | Point d'entrée pour les agents dans ce repo | Toujours — en premier |
| [`/progress.md`](../progress.md) | Tableau de bord vivant (phase, statut, blocages) | Pour savoir où en est le projet |
| [`/TODO.md`](../TODO.md) | Tâches différées hors scope courant | Pour ne pas oublier les sujets ouverts |
| [`/.devin/skills/cascade-self-config/SKILL.md`](../.devin/skills/cascade-self-config/SKILL.md) | Procédure du skill (6 phases) | Pour comprendre comment le skill fonctionne |
| [`/.devin/skills/cascade-self-config/references/`](../.devin/skills/cascade-self-config/references/) | Guides génériques + patterns dcr + templates | Quand on modifie le skill ou ses références |

## Architecture Decision Records

| ADR | Titre | Statut |
|---|---|---|
| [ADR-0001](decisions/0001-adopt-adr-0007-symlink-distribution.md) | Adoption d'ADR-0007 (distribution par symlinks) | Accepted |

## Scripts

| Script | Rôle |
|---|---|
| [`/scripts/install-skills.sh`](../scripts/install-skills.sh) | Installation globale du skill (Linux/macOS, symlinks) |
| [`/scripts/install-skills.ps1`](../scripts/install-skills.ps1) | Installation globale du skill (Windows, junctions) |

## Référence externe

- **ADR-0007 du projet `devin-conversations-retriever`** : décision originale sur la distribution des skills par symlinks et la suppression des global rules. Ce repo adopte cette décision via ADR-0001 local.
  - Chemin : `/home/julien/Sources/devin-conversations-retriever/docs/decisions/0007-skill-distribution-symlinks.md`
