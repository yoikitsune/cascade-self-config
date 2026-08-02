# ADR-0001: Adoption d'ADR-0007 (distribution par symlinks)

> Status: Accepted
> Date: 2026-08-02

## Context

Le projet `devin-conversations-retriever` (dcr) a formalisé dans son **ADR-0007** (2026-08-02) un pattern de distribution des skills globaux via symlinks, avec suppression des global rules. Ce pattern a été conçu pour être **général** — applicable à tout projet outilleur qui shippe des skills globaux.

`cascade-self-config` est explicitement nommé dans l'ADR-0007 comme le second projet outilleur qui a motivé la décision : les deux projets shippent des skills globaux et, avant ADR-0007, auraient dû partager un fichier `global_rules.md` commun — créant un couplage problématique (qui possède le fichier ? que se passe-t-il à la désinstallation ? comment scale à N outils ?).

Avant cette ADR, `cascade-self-config` était dans un état non conforme :

1. **Structure du repo** : `SKILL.md` et `references/` à la racine du repo, pas dans `.devin/skills/<name>/` (la structure canonique d'ADR-0007).
2. **Installation globale** : copie manuelle dans `~/.codeium/windsurf/skills/cascade-self-config/`, sans script d'installation, sans mécanisme de mise à jour. **Drift confirmé** : la copie globale datait de 2026-07-13 et parlait encore de `trajectory_search` (pré-dcr), alors que le repo avait été mis à jour pour utiliser `dcr`.
3. **`global_rules.md` à la racine du repo** : vestige contenant un bloc `<dcr_tool_awareness>` — exactement le type d'awareness qu'ADR-0007 déplace vers la description tier-1 du skill.
4. **Pas de scripts d'installation** : aucun `install-skills.sh` ou `.ps1`.

## Decision

**Adopter le pattern d'ADR-0007 du projet dcr.** Ce repo suit la même structure et les mêmes scripts canoniques que `devin-conversations-retriever`.

### Décisions concrètes

1. **Le skill vit dans `.devin/skills/cascade-self-config/`** (source canonique versionnée). `SKILL.md` et `references/` y sont déplacés depuis la racine du repo.

2. **L'installation globale crée un symlink** depuis `~/.codeium/windsurf/skills/cascade-self-config` vers `<repo>/.devin/skills/cascade-self-config`. Sur Linux/macOS : `ln -s`. Sur Windows : `mklink /J` (junction, pas besoin d'admin).

3. **`scripts/install-skills.sh` (Linux/macOS) et `scripts/install-skills.ps1` (Windows)** sont les commandes canoniques d'install/update/uninstall. Adaptés depuis les scripts de dcr, avec :
   - `SKILLS=(cascade-self-config)` — un seul skill
   - `CLI_BINARIES=()` — **vide** (ce projet ne shippe pas de CLI, contrairement à dcr qui shippe `dcr`)

4. **`global_rules.md` est supprimé du repo.** C'était un vestige obsolète, jamais installé nulle part, et contradictoire avec ADR-0007. L'awareness de `cascade-self-config` est porté par sa description tier-1 dans le frontmatter du `SKILL.md` (~100 tokens, chargé dans chaque session).

5. **Updates live** : éditer `SKILL.md` dans le repo se propage immédiatement à toutes les sessions via le symlink. `git pull` est le mécanisme de mise à jour.

6. **Uninstall** ne supprime que le symlink — le repo est intact.

### Ce qui change par rapport à l'état précédent

| Aspect | Avant | Après |
|---|---|---|
| Structure skill | `SKILL.md` à la racine | `.devin/skills/cascade-self-config/SKILL.md` |
| Installation globale | copie manuelle (drift) | symlink via `scripts/install-skills.sh` |
| Updates | re-copie manuelle | live via `git pull` |
| `global_rules.md` repo | vestige obsolète | supprimé |
| Scripts d'install | aucun | `install-skills.sh` + `.ps1` |

## Consequences

- **Positives** :
  - Fin du drift entre le repo et l'installation globale
  - Mises à jour live (éditer le repo = mise à jour immédiate)
  - Cohérence avec le projet dcr (même pattern, même type de scripts)
  - Suppression du couplage via `global_rules.md` partagé
  - Désinstallation propre (symlink uniquement)

- **Nécessite** :
  - Test empirique Windows de `install-skills.ps1` (les junctions — tâche différée, voir `TODO.md`)
  - Migration de l'installation globale existante (suppression de la copie stale + création du symlink)

## Evidence

Voir ADR-0007 du projet `devin-conversations-retriever` pour l'évidence complète (Vercel Labs Skills CLI, Devin Desktop v2026.5.6-1, docs Devin, standard AGENTS.md) :

`/home/julien/Sources/devin-conversations-retriever/docs/decisions/0007-skill-distribution-symlinks.md`

Cette ADR est une **adoption locale** d'une décision cross-projet — pas une redéfinition. Le raisonnement et l'évidence sont dans ADR-0007 ; ce document ne fait que l'appliquer à ce repo.

## Relations

- **Adopte** : ADR-0007 de `devin-conversations-retriever` (skill distribution via symlinks, global rule removal)
- **Supersede localement** : la pratique de copie manuelle du skill dans `~/.codeium/windsurf/skills/`
- **Supprime** : `global_rules.md` à la racine du repo (vestige obsolète)
