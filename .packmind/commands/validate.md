Exécute la skill `validation-workflow` dans l'ordre strict et bloquant :
**code quality → build → test → e2e**.

- À la première étape rouge : stopper, rapporter l'échec précis, proposer une
  correction (en respectant TDD si du code de production change), puis reprendre
  le pipeline depuis le début.
- Vérifier que la couverture du domaine reste à 100 % (lignes + branches).
- Terminer par le tableau de statut et la conclusion **VALIDÉ** ou **BLOQUÉ**.

Ne pas déclarer la tâche terminée tant que toutes les étapes ne sont pas vertes.
