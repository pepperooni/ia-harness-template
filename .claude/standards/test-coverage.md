# Règle — Couverture de test

La couverture est **différenciée par couche** (cohérent avec l'architecture
hexagonale). On ne vise pas un chiffre unique sur tout le dépôt : on protège
la valeur là où elle est.

## Politique de couverture

| Couche                     | Exigence de couverture                         |
|----------------------------|------------------------------------------------|
| **Domain** (cœur métier)   | **100 %** des lignes **et** des branches       |
| Ports (interfaces)         | Couverts indirectement via le domaine          |
| Adapters (infra, I/O)      | Souple — tests ciblés sur la logique d'adaptation |
| Glue / composition root    | Optionnel — couvert par les tests e2e          |

> Le 100 % du domaine est une **conséquence** du TDD bien appliqué, pas un objectif
> à atteindre après coup. Si du code de domaine n'est pas couvert, c'est qu'il a
> été écrit sans test rouge préalable → violation de la règle TDD.

## Qualité avant quantité

- Le pourcentage mesure ce qui est exécuté, pas ce qui est **vérifié**.
  Du code exécuté sans assertion pertinente = couverture trompeuse.
- Couvrir les **branches** et cas limites, pas seulement le chemin nominal.
- Pas de test écrit uniquement pour gonfler le chiffre.

## Interdits

- Ne pas baisser le seuil du domaine pour faire passer la CI : corriger le code.
- Ne pas exclure du domaine de la mesure de couverture pour contourner la règle.
- Toute exclusion de couverture doit être justifiée et **validée**, jamais implicite.

<!-- 🔧 À COMPLÉTER (par package) :
     - outil de mesure de couverture
     - seuils configurés dans l'outil (domain = 100% lignes + branches)
     - chemins exclus légitimes (généré, migrations…) avec justification -->

## Garde-fou pour l'agent

Après chaque cycle TDD, vérifier que le domaine reste à 100 %. Si la couverture
du domaine descend, l'annoncer immédiatement et proposer le test manquant avant
de continuer.
