# Règle — Clean code

Le code est écrit pour être lu. La lisibilité prime sur l'astuce. Ces principes
sont **agnostiques au langage** ; les conventions de style propres au langage
sont précisées dans le `CLAUDE.md` du package concerné.

## Nommage

- Noms intentionnels : une variable/fonction dit *ce qu'elle est* ou *ce qu'elle fait*.
- Pas d'abréviations obscures, pas de nom générique (`data`, `tmp`, `manager`).
- Le nom d'une fonction est un verbe d'action ; celui d'un booléen se lit comme une question.

## Fonctions

- Petites, **une seule chose**, un seul niveau d'abstraction par fonction.
- Peu de paramètres ; au-delà de 3, envisager un objet-valeur (et voir Mitosis).
- Pas d'effets de bord cachés : ce que fait la fonction est ce que dit son nom.
- Préférer le retour anticipé à l'imbrication profonde de conditions.

## Lisibilité & structure

- Pas de nombres ou chaînes magiques : constantes nommées.
- Pas de code mort ni de commentaires obsolètes ; supprimer plutôt que commenter.
- Le commentaire explique le **pourquoi**, jamais le **quoi** (le code dit le quoi).
- Cohérence : un seul style dans tout le dépôt, pas de mélange de conventions.

## Gestion d'erreur

- Échouer explicitement ; ne pas avaler silencieusement une erreur.
- Pas de valeur sentinelle ambiguë ; types/exceptions explicites selon le langage.

## Duplication

- DRY raisonné : factoriser la **connaissance** dupliquée, pas la coïncidence
  syntaxique. Deux bouts identiques par hasard ne se factorisent pas d'office.

## Garde-fou pour l'agent

À chaque ajout, faire une passe de relecture : nommage, taille de fonction,
duplication, magie. Signaler les écarts au lieu de les laisser passer. Toute
amélioration lourde de lisibilité se fait en phase Refactor, tests verts.

<!-- 🔧 À COMPLÉTER (par package) : linter/formatter retenu, règles de style
     spécifiques au langage, longueur de ligne, conventions d'import. -->
