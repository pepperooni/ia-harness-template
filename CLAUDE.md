# {{NOM_DU_PROJET}} — Contexte agent

> Fichier mémoire racine, chargé automatiquement par Claude Code à chaque session
> et traité comme **instructions faisant autorité**. Garder ce fichier court
> (< 200 lignes) : le détail vit dans les fichiers importés ci-dessous.

<!-- 🔧 À COMPLÉTER : 3-5 lignes décrivant la mission du produit et le périmètre du dépôt. -->

## Carte du monorepo

Avant toute action, situer le travail dans l'arborescence. Ne jamais placer de
logique métier hors de la couche `domain` (voir règle architecture hexagonale).

```
packages/
  <!-- 🔧 À COMPLÉTER : lister chaque package et sa responsabilité, ex. -->
  <!-- core-domain/      → règles métier pures, zéro dépendance technique     -->
  <!-- api/              → adaptateur entrant HTTP                            -->
  <!-- persistence/      → adaptateur sortant base de données                 -->
  <!-- ui-components/    → bibliothèque de composants                         -->
```

Chaque package possède son propre `CLAUDE.md` (contexte local + commandes
spécifiques au langage). Claude le charge à la demande en travaillant dans le sous-arbre.

## Règles non négociables

Ces règles s'appliquent à **tous** les projets et priment sur toute préférence
ponctuelle exprimée en conversation. En cas de conflit, demander avant de dévier.

@.claude/standards/tdd.md
@.claude/standards/hexagonal-architecture.md
@.claude/standards/mitosis.md
@.claude/standards/clean-code.md
@.claude/standards/test-coverage.md
@.claude/standards/observability.md

## Standards de livraison

@.claude/standards/definition-of-done.md
@.claude/standards/commit-convention.md

## Workflow de validation

Toute tâche de développement n'est considérée terminée qu'après passage **vert**
du pipeline de validation : `code quality → build → test → e2e`.
Détails et ordre d'exécution : voir la skill `validation-workflow`
(`.claude/skills/validation-workflow/SKILL.md`).

Commandes disponibles :
- `/validate`    — exécute le pipeline complet et bloque si une étape échoue
- `/new-feature` — démarre une fonctionnalité en boucle TDD + hexagonale
- `/commit`      — produit un commit conforme (Conventional Commits + clé JIRA)

## Posture de l'agent

- **Ne prendre aucune hypothèse structurante.** Si une décision touche
  l'architecture, le contrat d'une interface, le schéma de données ou un choix
  de dépendance : poser la question avant d'agir.
- Travailler par **petits incréments testés** ; ne jamais empiler du code non
  couvert (voir TDD + couverture).
- Avant de déclarer une tâche finie : exécuter `/validate` et coller le résultat.
