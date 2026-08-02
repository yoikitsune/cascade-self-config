# Outils disponibles pour ce projet — Capacités et Limites

> Ce fichier est projet-spécifique. Il est créé automatiquement par le skill `cascade-self-config` lors de la première invocation dans un projet. Adaptez-le avec les outils de votre projet.

## CLI [Nom de l'outil]

### Commandes valides
| Commande | Usage | Notes |
|---|---|---|
| commande1 | Usage 1 | Note 1 |
| commande2 | Usage 2 | Note 2 |

### Pièges connus
- Piège 1
- Piège 2

## MCP [Nom du MCP]

### Capacités
| Tool | Ce qu'il fait |
|---|---|
| tool1 | Description |
| tool2 | Description |

### Limites connues
- Limite 1
- Limite 2

### Bonnes pratiques
- Pratique 1
- Pratique 2

## Navigateur (Playwright)

### Cas d'usage
- Cas 1
- Cas 2

### Bonnes pratiques
- Préférer `browser_snapshot` à `browser_take_screenshot` pour l'interaction
- Consulter la console avec `browser_console_messages` pour les erreurs JS
- Vérifier le réseau avec `browser_network_requests` pour les appels API

## Outils Cascade internes

| Tool | Ce qu'il fait |
|---|---|
| `run_command` | Exécuter une commande CLI |
| `read_file` | Lire un fichier |
| `write_to_file` | Créer un fichier |
| `edit` / `multi_edit` | Modifier un fichier existant |
| `grep_search` | Rechercher dans le code |
| `code_search` | Recherche sémantique dans le code |
| `trajectory_search` | Rechercher dans une conversation passée |
| `search_web` | Recherche web |
| `read_url_content` | Lire le contenu d'une URL |
