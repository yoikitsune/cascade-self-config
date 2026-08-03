# Guide des Rules Devin Local

## Syntaxe

Une rule est un fichier markdown avec un frontmatter YAML obligatoire :

```yaml
---
trigger: always_on | model_decision | glob | manual
description: Description du contexte (requis pour model_decision/glob)
---
```

### Modes d'activation

| Trigger | Quand s'applique | Usage typique |
|---|---|---|
| `always_on` | Toujours chargé dans le contexte | Erreurs critiques qui peuvent survenir à tout moment sans signal contextuel |
| `model_decision` | Devin Local décide quand charger en fonction du contexte | Préférence par défaut pour économiser le contexte |
| `glob` | S'applique uniquement aux fichiers correspondant au pattern | Règles spécifiques à certains types de fichiers |
| `manual` | Jamais chargé automatiquement, invocation explicite via @rule-name | Documentation ou procédures rarement utilisées |

### Syntaxe glob

Le trigger `glob` utilise `globs` (pluriel) pour spécifier les patterns :

```yaml
---
trigger: glob
globs: **/*.test.ts
description: Règles pour les fichiers de test
---
```

Patterns valides : `*.js`, `src/**/*.ts`, `**/*.test.ts`, etc.

## Emplacements de stockage

### Rules projet
- `.devin/rules/*.md` (emplacement recommandé)
- `.windsurf/rules/*.md` (legacy, déprécié)

### Rules globales (utilisateur)
- `~/.codeium/windsurf/memories/global_rules.md` (fichier unique — chemin legacy, toujours lu par Devin Desktop)
- Via l'UI : Customizations → Edit → Rules → + Global

### Rules système (Enterprise)
- `/etc/devin/rules/` ou `/etc/windsurf/rules/`

### Discovery
- Workspace et sous-répertoires : tous les `.devin/rules/` trouvés
- Git : recherche jusqu'au git root dans les répertoires parents
- Multi-workspace : déduplication avec le chemin relatif le plus court

## Exemples

### always_on (erreur critique)

```yaml
---
trigger: always_on
---

Toujours vérifier les imports avant de les utiliser. Ne jamais inventer de noms de classes.
```

### model_decision (contextuel)

```yaml
---
trigger: model_decision
description: Quand l'utilisateur travaille avec Firebase CLI
---

Pour les logs Firebase, utiliser `firebase functions:log --project <id> | tail -N` plutôt que `--limit` (option inexistante).
```

### glob (fichiers spécifiques)

```yaml
---
trigger: glob
globs: **/*.dart
description: Pour les fichiers Dart dans le projet
---

Dans les fichiers Dart, utiliser uniquement les imports absolus `package:project/...` jamais les chemins relatifs.
```

## Best practices (documentation officielle)

- **Garder les rules simples, concises et spécifiques**. Les rules trop longues ou vagues peuvent confondre Devin Local.
- **Pas de règles génériques** (ex: "write good code") — déjà dans le training data de Devin Local.
- **Formater avec bullet points, numbered lists, et markdown** — plus facile à suivre qu'un long paragraphe.
- **XML tags pour grouper des règles similaires** :

```xml
<coding_guidelines>
- My project's programming language is python
- Use early returns when possible
- Always add documentation when creating new functions and classes
</coding_guidelines>
```

## Limites

- Rule : < 12 000 caractères
- Préférer `model_decision` à `always_on` pour économiser le contexte permanent
- Si > 6 rules `always_on`, envisager de convertir certaines en `model_decision`
- Pour les procédures complexes multi-étapes, créer un skill à la place
