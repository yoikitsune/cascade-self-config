# TODO — cascade-self-config & DCR integration

> Tâches en attente, hors code, à discuter et planifier ultérieurement.

## Déploiement & Distribution

- [ ] **Stratégie de déploiement cascade-self-config** — Comment distribuer le skill (manuel, script d'install, symlink, git clone vers `~/.codeium/windsurf/skills/` ?). Réfléchir à :
  - Comment garder le skill synchronisé entre le repo et l'installation locale
  - Comment gérer les mises à jour (git pull ? script ?)
  - Comment gérer les références projet-spécifiques (templates vs créés)
- [ ] **Rule globale `dcr-tool-awareness` hors repo** — La Rule vit dans `~/.codeium/windsurf/memories/global_rules.md` (config utilisateur, pas dans un repo git). Réfléchir à :
  - Faut-il la versionner dans un repo (cascade-self-config ou DCR) ?
  - Faut-il un script d'install qui l'écrit dans `global_rules.md` ?
  - Que faire si l'utilisateur a déjà du contenu dans `global_rules.md` (append vs overwrite) ?
  - Comment gérer les mises à jour de la Rule sans écraser les autres rules globales
