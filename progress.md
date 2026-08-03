# Progress — devin-self-config

> Dernière mise à jour : 2026-08-03 (ADR-0003 — mémoire projet dans `.devin/memory/`)

## Current Phase: Stabilisation post-migration

Le projet a migré de Cascade vers Devin Local (ADR-0002) puis a déplacé la mémoire projet vers `.devin/memory/` (ADR-0003). Toutes les phases actives sont terminées.

## Ce qui est fait

- [x] **ADR-0003 — Mémoire projet dans `.devin/memory/`** : SKILL.md mis à jour (Phase 0b avec migration automatique, tous les chemins mis à jour), ADR créé. La migration des fichiers existants dans les projets utilisateurs se fera automatiquement à la prochaine invocation du skill (Phase 0b).
- [x] **ADR-0002 — Migration Cascade → Devin Local** : renommage du skill `cascade-self-config` → `devin-self-config`, migration du chemin global `~/.codeium/windsurf/skills/` → `~/.config/devin/skills/` (XDG), scripts mis à jour avec cleanup automatique des anciens chemins, SKILL.md réécrit (Cascade → Devin Local, 4 URLs de doc au lieu de 3, doc Devin Local ajoutée), toutes les références et la doc mises à jour
- [x] **Phase 2 — Documentation projet** : `AGENTS.md`, `docs/index.md`, `docs/decisions/0001-adopt-adr-0007-symlink-distribution.md`, `progress.md` créés, `TODO.md` nettoyé
- [x] **Phase 1 — Restructuration selon ADR-0007** : skill déplacé dans `.devin/skills/devin-self-config/`, `scripts/install-skills.{sh,ps1}` créés, `global_rules.md` supprimé, installation globale migrée (copie stale → symlink validé)
- [x] **Phase 3 — Mise à jour du SKILL.md** : section "Architecture hybride" réécrite pour le modèle symlink (ADR-0001), prérequis `dcr` explicite avec procédure d'installation, référence à `cascade-self-automation` supprimée (skill inexistant), `references/dcr-diagnostic-patterns.md` mis à jour (`dcr` sur PATH via wrapper, `dcr sync` manuel retiré — auto-sync)
- [x] **Phase 4 — Test et validation** : `--list` validé, résolution symlink validée, mise à jour live validée (marker temporaire propagé instantanément), cycle remove/install validé
- [x] Intégration `dcr` dans le skill (commit `232cdb6`) — Phase 1 du SKILL.md utilise `dcr` pour l'analyse de conversations
- [x] Support Windows dans le SKILL.md (commit `6084446`) — chemins et commandes PowerShell

## Ce qui est en cours

Rien — toutes les phases sont terminées.

## Ce qui est prévu

Rien — toutes les phases sont terminées.

## Ce qui est bloqué

Rien actuellement bloqué.

## Architecture

- **Type** : Skill pur (markdown + références, pas de code)
- **Distribution** : symlink global via `scripts/install-skills.{sh,ps1}` (per ADR-0001)
- **Chemin global** : `~/.config/devin/skills/` (XDG-convention, per ADR-0002) — les scripts nettoient automatiquement l'ancien chemin `~/.codeium/windsurf/skills/`
- **Dépendance forte** : `dcr` (Devin Conversations Retriever) pour l'analyse de conversations en Phase 1 — doit être installé globalement via `devin-conversations-retriever/scripts/install-skills.sh`
- **Awareness** : porté par la description tier-1 du skill (~100 tokens), pas par une global rule
- **Agent cible** : Devin Local (successeur de Cascade depuis le 1er juillet 2026)

## ADRs

| ADR | Titre | Statut |
|---|---|---|
| [ADR-0001](docs/decisions/0001-adopt-adr-0007-symlink-distribution.md) | Adoption d'ADR-0007 (distribution par symlinks) | Accepted |
| [ADR-0002](docs/decisions/0002-migrate-cascade-to-devin-local.md) | Migration Cascade → Devin Local (renommage + chemins) | Accepted |
| [ADR-0003](docs/decisions/0003-memory-in-devin-memory.md) | Mémoire projet dans `.devin/memory/` au lieu de `.devin/skills/` | Accepted |
