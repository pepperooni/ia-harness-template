Implémente la fonctionnalité demandée : **$ARGUMENTS**

Procéder ainsi, sans prendre de décision structurante sans validation :

1. **Cadrage** — reformuler le besoin, identifier la/les couche(s) hexagonale(s)
   impactée(s). Si un contrat de port, un schéma de données ou une dépendance
   nouvelle est nécessaire : **le proposer et demander avant d'écrire du code.**
2. **Boucle TDD** par plus petits incréments possibles :
   - Red : écrire un test qui échoue (montrer le message d'échec).
   - Green : code minimal pour passer au vert.
   - Refactor : nettoyer + appliquer Mitosis si une unité devient trop chargée,
     à tests verts et couverture constante.
3. **Placement** — pour chaque fichier, annoncer sa couche (domain / port / adapter)
   et respecter la règle de dépendance vers l'intérieur.
4. **Couverture** — domaine à 100 % (lignes + branches).
5. **Validation** — lancer `/validate`. Ne pas conclure tant que ce n'est pas vert.
6. **Done** — vérifier la Definition of Done et préparer le `/commit`.
