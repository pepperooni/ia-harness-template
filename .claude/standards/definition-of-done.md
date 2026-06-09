# Standard — Definition of Done

Une tâche de développement n'est **terminée** que si **toutes** ces conditions
sont vraies. C'est le contrat qui rend la délégation à un agent fiable : tant
que la liste n'est pas verte, le travail continue.

## Checklist

- [ ] Le besoin a été implémenté **via le cycle TDD** (test rouge → vert → refactor).
- [ ] Le code respecte l'**architecture hexagonale** (dépendances vers le domaine,
      aucun détail technique dans le cœur).
- [ ] Les modules trop chargés ont été **divisés** (Mitosis) ; cohésion vérifiée.
- [ ] Le code respecte les principes **clean code** (nommage, fonctions courtes,
      pas de duplication ni de magie).
- [ ] La **couverture du domaine est à 100 %** (lignes + branches) ; couverture
      des adaptateurs cohérente avec leur logique.
- [ ] Le **pipeline de validation est vert** : `code quality → build → test → e2e`
      (voir skill `validation-workflow`). Résultat collé dans la réponse.
- [ ] Le **commit** est conforme (Conventional Commits + clé JIRA + description
      fonctionnelle) et passe le hook `commit-msg`.
- [ ] Les points fonctionnels significatifs émettent des **logs structurés**
      (`action` + contexte + niveau adapté) ; aucun log technique creux.
- [ ] Si un framework d'observabilité est présent (détecté par grep sur les
      dépendances) : un **span** et/ou une **métrique** ont été ajoutés pour
      chaque nouvelle fonctionnalité ou processus.
- [ ] Aucune décision structurante n'a été prise sans validation préalable.

## Sortie attendue de l'agent en fin de tâche

1. Résumé fonctionnel de ce qui a été fait.
2. Liste des fichiers créés/modifiés avec leur **couche** (domain / port / adapter).
3. Sortie complète de `/validate` (statut de chaque étape).
4. Le message de commit proposé.
