# TODO — devin-self-config

> Tâches différées, hors scope courant. Pour le statut live du projet, voir `progress.md`.

## Installation & Distribution

- [ ] **Test empirique Windows de `install-skills.ps1`** — Lancer `.\scripts\install-skills.ps1` sur une machine Windows pour confirmer que Devin suit les junctions (`mklink /J`) et que le chemin `%APPDATA%\devin\skills\` est bien découvert par Devin Local. Tâche différée — la version Linux/macOS est testée et validée.

## Skills obsolètes

- [ ] **Archiver `evaluateur`** — Le skill `evaluateur` (à `~/.codeium/windsurf/skills/evaluateur/`) est obsolète. À archiver : supprimer la copie globale, et si une trace historique est souhaitée, la documenter dans un ADR. Tâche différée — ne pas traiter maintenant.

## Évolutions futures possibles

- [ ] **Réévaluer `devin-self-automation`** — L'ancienne référence à `cascade-self-automation` (skill inexistant) a été renommée conceptuellement en `devin-self-automation` lors de la migration. Soit le créer (nouvel skill pour les automatisations projet), soit retirer définitivement la référence. À discuter.
- [ ] **Explorer les subagents Devin Local** — Devin Local supporte les subagents (foreground/background). Le skill pourrait potentiellement utiliser des subagents pour paralléliser l'analyse en Phase 1. À évaluer.
- [ ] **Renommer le repo GitHub** — Le remote est actuellement `https://github.com/yoikitsune/cascade-self-config.git`. À renommer en `devin-self-config` via `gh repo rename` (ou manuellement sur GitHub). Mettre à jour le remote local avec `git remote set-url origin`.
