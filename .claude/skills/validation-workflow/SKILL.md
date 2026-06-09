---
name: 'validation-workflow'
description: 'Exécute le pipeline de validation obligatoire avant de considérer toute tâche de développement terminée. À utiliser systématiquement en fin de tâche, avant un commit, ou dès que l''utilisateur demande de valider/vérifier le travail. Ordre strict et bloquant : code quality (lint) → build → test → e2e.'
---

# Skill — Workflow de validation

Pipeline **séquentiel et bloquant**. Chaque étape doit être **verte** avant de
passer à la suivante. À la première étape rouge : **stopper**, rapporter
précisément l'échec, corriger (en respectant TDD si du code change), puis
**reprendre le pipeline depuis le début**.

## Étapes (ordre imposé)

### 0. Infrastructure (si requise)
Si une stack d'infrastructure est nécessaire pour {{NEEDS_SERVICES_FOR}}, la monter
**avant** ces étapes. Attendre que les services soient « healthy » avant de poursuivre.
Si `{{CMD_SERVICES_UP}}` vaut `none`, sauter cette étape (et le teardown).
```
<!-- 🔧 À COMPLÉTER : commande de démarrage de la stack, ou `none` -->
{{CMD_SERVICES_UP}}
```

### 1. Code quality
Analyse statique : lint + format + règles de style. Aucun avertissement bloquant.
```
<!-- 🔧 À COMPLÉTER (par package/langage) : commande lint+format -->
{{CMD_LINT}}
```

### 2. Build
Compilation / build de tous les packages impactés. Zéro erreur, zéro warning bloquant.
```
<!-- 🔧 À COMPLÉTER : commande de build -->
{{CMD_BUILD}}
```

### 3. Test
Tests unitaires + intégration. Tous verts. Vérifier la **couverture du domaine
à 100 %** (lignes + branches) — voir règle de couverture.
```
<!-- 🔧 À COMPLÉTER : commande de test + couverture -->
{{CMD_TEST}}
```

### 4. E2E
Tests de bout en bout sur les parcours critiques. Tous verts.
```
<!-- 🔧 À COMPLÉTER : commande e2e -->
{{CMD_E2E}}
```

## Règles d'exécution

- Ne **jamais** sauter une étape, même si « le changement est minime ».
- Ne **jamais** déclarer une tâche terminée avec une étape rouge ou non exécutée.
- Ne pas masquer un échec (skip, `--force`, désactivation de test) pour faire
  passer le pipeline : corriger la cause, ou **demander** si le besoin est ambigu.
- En monorepo, scoper aux packages impactés est acceptable **si** l'outillage
  garantit que rien d'autre n'est cassé ; sinon, valider l'ensemble.
- La stack montée à l'étape 0 doit **toujours** être démontée en fin de pipeline
  — succès **comme** échec (sémantique `finally`) — via la commande ci-dessous.
  Ne jamais laisser de conteneurs orphelins. Si `{{CMD_SERVICES_UP}}` vaut `none`,
  il n'y a rien à démonter.
  ```
  <!-- 🔧 À COMPLÉTER : commande d'arrêt de la stack, ou `none` -->
  {{CMD_SERVICES_DOWN}}
  ```

## Rapport de sortie

Produire un tableau de statut, par exemple :

| Étape         | Statut | Détail                          |
|---------------|--------|---------------------------------|
| Infra/stack   | ✅/❌/— | montée puis démontée / non requise |
| Code quality  | ✅/❌   | …                               |
| Build         | ✅/❌   | …                               |
| Test          | ✅/❌   | … (couverture domaine : ___ %)  |
| E2E           | ✅/❌   | …                               |

Conclure explicitement : **VALIDÉ** (tout vert) ou **BLOQUÉ** (+ cause + prochaine action).