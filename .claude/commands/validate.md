---
description: 'Exécuter le workflow de validation de façon strictement séquentielle (code quality → build → test → e2e), en stoppant au premier échec pour le diagnostiquer et proposer une correction puis relancer depuis le début, afin de garantir un pipeline fiable avec une couverture domaine à 100 % avant de conclure VALIDÉ ou BLOQUÉ lors de toute livraison ou modification de code.'
---

Exécute la skill `validation-workflow` dans l'ordre strict et bloquant :
**(infra si requise) → code quality → build → test → e2e**.

- Si la skill définit une commande de montage de stack (`CMD_SERVICES_UP` ≠ `none`),
  la monter à l'étape 0 avant les étapes concernées, et **toujours** la démonter
  en fin de pipeline (`CMD_SERVICES_DOWN`), succès comme échec.
- À la première étape rouge : **démonter d'abord la stack** si elle a été montée
  (pas de conteneurs orphelins), puis stopper, rapporter l'échec précis, proposer
  une correction (en respectant TDD si du code de production change), puis
  reprendre le pipeline depuis le début (qui remontera la stack à l'étape 0).
- Vérifier que la couverture du domaine reste à 100 % (lignes + branches).
- Terminer par le tableau de statut et la conclusion **VALIDÉ** ou **BLOQUÉ**.

Ne pas déclarer la tâche terminée tant que toutes les étapes ne sont pas vertes.
