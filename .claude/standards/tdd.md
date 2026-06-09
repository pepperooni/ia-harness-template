# Règle — TDD always

Le développement suit **strictement** le cycle test-first. Aucune ligne de code
de production n'est écrite sans qu'un test l'exige au préalable.

## Cycle Red → Green → Refactor

1. **Red** — écrire un test qui décrit le comportement attendu. Le lancer, vérifier
   qu'il **échoue** pour la bonne raison (assertion, pas erreur de compilation/import).
2. **Green** — écrire le minimum de code de production pour faire passer le test.
   Pas d'anticipation, pas de généralisation prématurée.
3. **Refactor** — nettoyer code **et** tests à couverture constante, tests toujours verts.

Répéter par incréments les plus petits possibles.

## Règles dures

- Ne jamais écrire de code de production en réponse à autre chose qu'un test rouge.
- Ne jamais ajouter plusieurs comportements dans un même cycle.
- Un test qui ne peut pas échouer n'est pas un test : vérifier le Red.
- Ne pas modifier un test pour le faire passer ; corriger le code, ou demander si
  le test exprime un mauvais besoin.
- Les tests sont du code de production : lisibilité, nommage et clean code s'y appliquent.

## Structure d'un test

Pattern **Arrange / Act / Assert** (ou Given / When / Then). Un seul comportement
vérifié par test. Nom du test = phrase décrivant le comportement attendu.

<!-- 🔧 À COMPLÉTER (par langage, dans le CLAUDE.md du package) :
     - framework de test retenu
     - convention de nommage des fichiers de test
     - commande pour lancer un test isolé -->

## Vérification attendue de l'agent

À chaque incrément, montrer dans l'ordre : (1) le test rouge et son message
d'échec, (2) le code minimal, (3) le passage au vert. Ne pas sauter d'étape.
