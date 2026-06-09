# Règle — Architecture hexagonale first

Toute fonctionnalité est conçue selon le modèle ports & adapters. Le **domaine**
est le centre : il ne connaît aucun détail technique (framework, base de données,
HTTP, file system, horloge, réseau).

## Les trois couches

- **Domain (cœur)** — entités, objets-valeur, règles métier, cas d'usage.
  Dépendances techniques : **zéro**. Pure logique. C'est ici que vit la valeur.
- **Ports** — interfaces définies *par le domaine* pour exprimer ses besoins
  (port entrant = cas d'usage exposé ; port sortant = dépendance requise, ex.
  `RepositoryPort`, `ClockPort`, `NotificationPort`).
- **Adapters** — implémentations concrètes des ports, en périphérie.
  Entrants : HTTP, CLI, événements. Sortants : base de données, API externe, etc.

## Règle de dépendance (absolue)

Les dépendances pointent **toujours vers l'intérieur**.
`adapter → port → domain`. Jamais l'inverse.

- Le domaine n'importe **jamais** un adaptateur ni une lib technique.
- Un adaptateur dépend d'un port (interface), pas d'un autre adaptateur.
- L'injection se fait par les bords (composition root), pas dans le domaine.

## Conséquences pratiques

- Le domaine est testable sans infrastructure (tests unitaires rapides, sans I/O).
- Remplacer une base de données ou un framework = remplacer un adaptateur, sans
  toucher au domaine.
- Si un test de domaine a besoin d'un mock technique → l'abstraction de port manque.

## Heuristique de placement

> « Est-ce que cette règle resterait vraie si on changeait de base de données,
> de framework web ou d'UI ? » Si oui → `domain`. Sinon → `adapter`.

## Garde-fous pour l'agent

- Avant de créer un fichier, annoncer sa couche (domain / port / adapter).
- Détecter et signaler tout import technique qui remonterait dans le domaine.
- Ne pas inventer un port ou un contrat d'interface sans validation : **demander**.

<!-- 🔧 À COMPLÉTER (par package) : mapping concret couche → dossier/module
     du langage, et nom de la composition root. -->
