# Standard — Convention de commit

Tous les commits suivent **Conventional Commits**, enrichis d'une **clé JIRA
obligatoire** et d'une **description fonctionnelle**. Le respect est garanti par
un hook `commit-msg` (voir `scripts/hooks/commit-msg`).

## Format

```
<type>(<scope>): <résumé impératif court>  [<CLE-JIRA>]

<description fonctionnelle : QUOI change pour l'utilisateur et POURQUOI>

<footer optionnel : BREAKING CHANGE, refs, co-authors>
```

### Règles du sujet
- `type` ∈ `feat | fix | refactor | test | docs | chore | build | ci | perf | style`
- `scope` = package ou module concerné (optionnel mais recommandé en monorepo)
- Résumé à l'**impératif présent**, sans majuscule initiale, sans point final, ≤ 72 car.
- **Clé JIRA obligatoire**, format `{{PROJET}}-<number>` (ex. `ABC-1234`).

<!-- 🔧 À COMPLÉTER : préfixe(s) de projet JIRA autorisé(s), ex. ABC, PAY, CORE. -->

### Règle du corps
- Une **description fonctionnelle** est obligatoire (pas seulement technique) :
  ce que le changement apporte du point de vue métier / utilisateur, et la raison.
- Lignes ≤ 100 caractères.

## Exemple

```
feat(billing): calcule la TVA selon le pays de facturation  [PAY-1842]

Les commandes hors UE sont désormais facturées hors taxe. Aligne le calcul
sur la nouvelle règle comptable entrée en vigueur ce trimestre.

Refs: PAY-1842
```

## Garde-fou pour l'agent

Lors d'un `/commit` : exiger la clé JIRA ; si elle est absente, **la demander**
plutôt que d'en inventer une. Rédiger une description fonctionnelle réelle, pas
une paraphrase du diff technique.
