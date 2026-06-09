# Règle — Mitosis (division continue des modules)

Métaphore biologique : comme une cellule qui se divise dès qu'elle devient trop
grosse, un module/fichier/fonction qui dépasse sa responsabilité doit être
**scindé** plutôt que de continuer à grossir. La mitose est un acte de design
**proactif**, pas un nettoyage de fin de projet.

## Principe

Une unité de code = **une seule responsabilité**, **une seule raison de changer**.
Dès qu'une unité commence à porter deux intentions, on la divise en deux unités
cohésives, chacune avec un nom qui décrit sa raison d'être.

## Signaux de division (déclencheurs)

Diviser dès qu'un de ces signaux apparaît :

- Le nom de l'unité contient « et / manager / utils / helper / misc ».
- On hésite sur où ranger un nouveau bout de code → la frontière est floue.
- Deux groupes de fonctions ne partagent aucun état commun dans le même fichier.
- Un changement fonctionnel oblige à modifier des parties non liées.
- La couverture exige des cas de test sans rapport entre eux dans le même module.

<!-- 🔧 À COMPLÉTER (par projet) : seuils chiffrés indicatifs si souhaité
     (longueur de fichier/fonction, nb de paramètres, complexité cyclomatique).
     Les laisser comme garde-fous, pas comme règles aveugles. -->

## Comment diviser proprement

- Extraire vers une nouvelle unité **cohésive**, nommée par sa responsabilité.
- Respecter l'architecture hexagonale : une division ne doit pas faire fuiter du
  technique dans le domaine.
- La division est faite en phase **Refactor** du cycle TDD, à tests verts et
  couverture constante.
- Chaque unité issue de la division reste indépendamment testable.

## Anti-pattern

Ne pas confondre mitose et fragmentation : ne pas créer des micro-fichiers
anémiques qui n'ont de sens qu'ensemble. La division doit augmenter la cohésion,
pas seulement réduire la taille.

## Garde-fou pour l'agent

Quand une unité approche d'un signal de division, le signaler et proposer le
découpage **avant** d'y ajouter du code. Ne pas restructurer en profondeur sans
validation.
