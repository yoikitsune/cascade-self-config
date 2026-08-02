# TODO — cascade-self-config

> Tâches différées, hors scope courant. Pour le statut live du projet, voir `progress.md`.

## Installation & Distribution

- [ ] **Test empirique Windows de `install-skills.ps1`** — Lancer `.\scripts\install-skills.ps1` sur une machine Windows pour confirmer que Devin suit les junctions (`mklink /J`). Tâche différée — la version Linux/macOS est testée et validée.

## Skills obsolètes

- [ ] **Archiver `evaluateur`** — Le skill `evaluateur` (à `~/.codeium/windsurf/skills/evaluateur/`) est obsolète. À archiver : supprimer la copie globale, et si une trace historique est souhaitée, la documenter dans un ADR. Tâche différée — ne pas traiter maintenant.

## Évolutions futures possibles

- [ ] **Réévaluer `cascade-self-automation`** — Le SKILL.md référence un skill `cascade-self-automation` qui n'existe pas. Soit le créer (nouvel skill pour les automatisations projet), soit retirer définitivement la référence. À discuter.
