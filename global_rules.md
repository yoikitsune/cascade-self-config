<dcr_tool_awareness>
L'outil `dcr` (Devin Conversations Retriever) permet de chercher dans l'historique des conversations Cascade de façon bien plus puissante que `trajectory_search` (recherche full-text FTS5, filtres par projet/date, export, archivage permanent).

- Chemin du projet : `/home/julien/Sources/devin-conversations-retriever/`
- Binaire : `/home/julien/Sources/devin-conversations-retriever/.venv/bin/dcr`
- Usage : `dcr --help` pour la liste complète des commandes
- Commandes principales : `dcr search "<query>"`, `dcr list`, `dcr show <id>`, `dcr export <id>`, `dcr status`, `dcr html`
- **Auto-sync** : toutes les commandes font un sync automatique avant de s'exécuter (sauf `--no-sync`). Ne **pas** lancer `dcr sync` manuellement avant une autre commande — c'est redondant
- La DB SQLite est à `~/.local/share/dcr/dcr.db` (archive permanente — les conversations supprimées par Windsurf restent recherchables)
- Préférer `dcr` à `trajectory_search` quand il faut : chercher across conversations, filtrer par projet/date, exporter, ou accéder à des conversations archivées
- **ATTENTION — confusion d'ID** : `trajectory_search` attend un **Cascade UUID** (format `586311a4-59b9-444f-bca2-259dcce5f214`), PAS un ID numérique `dcr` (145). Utiliser `dcr show <id>` ou `dcr export <id>` avec les IDs numériques. Ne jamais passer un ID numérique à `trajectory_search`.
</dcr_tool_awareness>