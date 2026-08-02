# Patterns diagnostiques avec `dcr` — cascade-self-config

> Ce fichier contient les procédures spécifiques pour utiliser `dcr` dans le contexte du diagnostic de comportement Cascade. Il est chargé par le skill `cascade-self-config` lors de la Phase 1.
>
> Pour la documentation générique de `dcr`, utiliser `dcr --help` ou consulter `/home/julien/Sources/devin-conversations-retriever/README.md`.

## Prérequis

- `dcr` est installé à `/home/julien/Sources/devin-conversations-retriever/.venv/bin/dcr`
- La DB est à `~/.local/share/dcr/dcr.db`
- Si la DB n'existe pas ou est vide, lancer `dcr sync` en premier

## Pattern 1 — Analyser une conversation spécifique

**Quand** : l'utilisateur fournit une conversation à analyser (mode explicite ou libre).

```bash
# 1. Sync pour s'assurer que la conversation est dans la DB
dcr sync

# 2. Lister les conversations récentes pour trouver l'ID
dcr list -l 10

# 3. Afficher la conversation complète (prefix d'ID suffisant)
dcr show <id_prefix>

# 4. Exporter en markdown pour analyse approfondie
dcr export <id_prefix> -o /tmp/conversation-analysis.md
```

> **Avantage sur `trajectory_search`** : `dcr show` retourne la conversation complète sans limite de 50 chunks. `dcr export` produit un markdown structuré par rounds/steps, plus facile à analyser que le format fragmenté de `trajectory_search`.

## Pattern 2 — Rechercher des erreurs récurrentes across conversations

**Quand** : l'utilisateur signale un problème récurrent ou veut identifier des patterns d'erreurs.

```bash
# Sync d'abord
dcr sync

# Rechercher par mot-clé d'erreur
dcr search "command not found"
dcr search "error: "
dcr search "failed to"

# Filtrer par projet
dcr search "erreur" -p <project_name>

# Rechercher des patterns spécifiques
dcr search "trajectory_search"
dcr search "run_command"
dcr search "MCP"
```

## Pattern 3 — Comparer deux conversations

**Quand** : l'utilisateur veut comparer une session réussie avec une session échouée.

```bash
dcr sync

# Exporter les deux conversations
dcr export <id1_prefix> -o /tmp/conv1.md
dcr export <id2_prefix> -o /tmp/conv2.md

# Lire et comparer manuellement les deux exports
```

## Pattern 4 — Diagnostiquer un problème de sélection d'outil

**Quand** : Cascade a utilisé le mauvais outil (MCP au lieu de CLI, navigateur au lieu de script, etc.).

```bash
dcr sync

# Chercher les mentions d'outils dans les conversations
dcr search "MCP"
dcr search "browser_navigate"
dcr search "run_command"
dcr search "grep_search"

# Voir les conversations où ces outils ont été mentionnés
dcr list -l 20
dcr show <id_prefix>
```

## Pattern 5 — Identifier des étapes gaspillées

**Quand** : Cascade a fait trop d'étapes pour une tâche simple.

```bash
dcr sync

# Exporter la conversation
dcr export <id_prefix> -o /tmp/conv.md

# Analyser le markdown exporté :
# - Compter les rounds (séparés par ## Round N)
# - Identifier les étapes de recherche vs action
# - Repérer les allers-retours inutiles
```

## Pattern 6 — Vérifier si une erreur a déjà été diagnostiquée

**Quand** : avant de créer une nouvelle correction, vérifier si le même pattern d'erreur existe dans d'autres conversations.

```bash
dcr sync

# Rechercher le pattern d'erreur
dcr search "<pattern_specifique>"

# Voir dans quelles conversations ce pattern apparaît
dcr list -l 20
```

## Interprétation des résultats

### Format de sortie `dcr search`

```
[conversation_id] [date] [project] [round/step] [snippet]
```

- **conversation_id** : prefix utilisable avec `dcr show`
- **project** : projet dans lequel la conversation a eu lieu
- **snippet** : extrait autour du match, avec `<query>` surligné

### Format de sortie `dcr show`

Affiche :
- Métadonnées (ID, date, projet, modèle, titre)
- Rounds (regroupements logiques)
- Steps (actions individuelles : user, assistant, tool_call, tool_result, checkpoint)
- Checkpoints (résumés de session)

### Format de sortie `dcr export`

Markdown structuré :
- `# Conversation <id>` — en-tête
- `## Round N` — chaque round
- `### Step N (type)` — chaque step avec son type et contenu
- `### Checkpoint N` — checkpoints

## Limites et fallbacks

- Si `dcr` n'est pas installé ou la DB est vide → fallback sur `trajectory_search`
- Si la conversation recherchée n'est pas dans la DB → lancer `dcr sync` puis réessayer
- Si la conversation a été supprimée par Windsurf mais est dans la DB → `dcr show` fonctionne (archive permanente)
- `dcr search` ne fait pas de recherche sémantique (FTS5 keyword only) — pour une recherche sémantique, utiliser `trajectory_search` en complément
