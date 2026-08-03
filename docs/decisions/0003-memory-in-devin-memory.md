# ADR-0003: Mémoire projet dans `.devin/memory/` au lieu de `.devin/skills/`

> Status: Accepted
> Date: 2026-08-03

## Context

Le skill `devin-self-config` utilise deux fichiers de mémoire projet-spécifique :

- `diagnostic-catalog.md` — catalogue des erreurs déjà diagnostiquées et corrigées dans un projet
- `project-tooling.md` — inventaire des outils disponibles (CLI, MCP, navigateur) et leurs limites connues

Ces fichiers étaient stockés dans `<projet>/.devin/skills/devin-self-config/references/`. Deux problèmes ont été identifiés :

1. **Sémantique incorrecte** : `.devin/skills/` est réservé aux définitions de skills (procédures). Ces fichiers sont de la **mémoire accumulée** (état persistant), pas des skills. Les mettre dans `.devin/skills/` crée une confusion sur la nature des fichiers.

2. **Débris legacy** : certains projets utilisateurs ont encore ces fichiers sous l'ancien nom de dossier `cascade-self-config` (avant la migration ADR-0002), c'est-à-dire dans `<projet>/.devin/skills/cascade-self-config/references/`. Le skill ne détectait ni ne migrait pas ces anciens emplacements.

## Decision

### 1. Déplacer la mémoire projet vers `.devin/memory/`

Les fichiers de mémoire projet-spécifiques vivent désormais dans :

```
<projet>/.devin/memory/devin-self-config/
  ├── diagnostic-catalog.md
  └── project-tooling.md
```

**Justifications** :
- `.devin/memory/` = état accumulé par les skills (sémantique claire)
- Namespaced par skill (`devin-self-config/`) pour éviter les collisions si d'autres skills adoptent ce pattern
- `.devin/skills/` reste réservé aux vrais skills du projet
- Les templates restent dans le skill global (`references/` du repo) — seuls les fichiers d'état déménagent

### 2. Migration automatique en Phase 0b

Le skill détecte et migre automatiquement les anciens emplacements lors de la Phase 0b :

| Ancien chemin | Nouveau chemin |
|---|---|
| `<projet>/.devin/skills/cascade-self-config/references/diagnostic-catalog.md` | `<projet>/.devin/memory/devin-self-config/diagnostic-catalog.md` |
| `<projet>/.devin/skills/cascade-self-config/references/project-tooling.md` | `<projet>/.devin/memory/devin-self-config/project-tooling.md` |
| `<projet>/.devin/skills/devin-self-config/references/diagnostic-catalog.md` | `<projet>/.devin/memory/devin-self-config/diagnostic-catalog.md` |
| `<projet>/.devin/skills/devin-self-config/references/project-tooling.md` | `<projet>/.devin/memory/devin-self-config/project-tooling.md` |

La migration :
- Crée `<projet>/.devin/memory/devin-self-config/` si nécessaire
- Déplace les fichiers existants
- Supprime les dossiers `references/` vides après migration
- Supprime les dossiers `cascade-self-config/` ou `devin-self-config/` vides dans `.devin/skills/` après migration

## Consequences

- **Positives** : séparation claire entre procédures (skills) et état (memory) ; détection automatique des débris legacy
- **Négatives** : tous les projets existants doivent être migrés (automatique à la prochaine invocation du skill)
- **Compatibilité** : le skill gère les trois emplacements possibles (cascade-self-config/references, devin-self-config/references, memory/devin-self-config) avec migration transparente

## Relations

- **Supersede** : le chemin `<projet>/.devin/skills/devin-self-config/references/` pour les fichiers de mémoire (issu d'ADR-0002)
- **Conserve** : les templates globaux dans `references/` du repo (ils ne bougent pas)
- **Ajoute** : le pattern `.devin/memory/<skill-name>/` pour la mémoire projet-spécifique
