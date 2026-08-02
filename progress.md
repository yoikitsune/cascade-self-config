# Progress — cascade-self-config

> Dernière mise à jour : 2026-08-02 (ADR-0001 — adoption ADR-0007, restructuration du repo)

## Current Phase: Mise en conformité ADR-0007

Le projet adopte le pattern de distribution par symlinks formalisé dans l'ADR-0007 du projet `devin-conversations-retriever`. Cette mise en conformité se fait en 4 phases.

## Ce qui est fait

- [x] **Phase 2 — Documentation projet** : `AGENTS.md`, `docs/index.md`, `docs/decisions/0001-adopt-adr-0007-symlink-distribution.md`, `progress.md` créés, `TODO.md` nettoyé
- [x] Intégration `dcr` dans le skill (commit `232cdb6`) — Phase 1 du SKILL.md utilise `dcr` pour l'analyse de conversations
- [x] Support Windows dans le SKILL.md (commit `6084446`) — chemins et commandes PowerShell

## Ce qui est en cours

- [ ] **Phase 1 — Restructuration selon ADR-0007** : déplacer le skill dans `.devin/skills/`, créer `scripts/install-skills.{sh,ps1}`, supprimer `global_rules.md`, migrer l'installation globale (copie stale → symlink)

## Ce qui est prévu

- [ ] **Phase 3 — Mise à jour du SKILL.md** : section "Architecture hybride" réécrite pour le modèle symlink, prérequis `dcr` explicite, suppression de la référence à `cascade-self-automation` (skill inexistant), mise à jour de `references/dcr-diagnostic-patterns.md`
- [ ] **Phase 4 — Test et validation** : test d'installation, test de mise à jour live, commit final

## Ce qui est bloqué

Rien actuellement bloqué.

## Architecture

- **Type** : Skill pur (markdown + références, pas de code)
- **Distribution** : symlink global via `scripts/install-skills.{sh,ps1}` (per ADR-0001)
- **Dépendance forte** : `dcr` (Devin Conversations Retriever) pour l'analyse de conversations en Phase 1 — doit être installé globalement via `devin-conversations-retriever/scripts/install-skills.sh`
- **Awareness** : porté par la description tier-1 du skill (~100 tokens), pas par une global rule

## ADRs

| ADR | Titre | Statut |
|---|---|---|
| [ADR-0001](docs/decisions/0001-adopt-adr-0007-symlink-distribution.md) | Adoption d'ADR-0007 (distribution par symlinks) | Accepted |
