# Harness agent IA — guide d'installation

Template de contexte structuré pour déléguer des tâches de développement à un
agent IA (cible : **Claude Code**) avec un niveau de confiance élevé. Il encode
des **règles non négociables**, des **standards de livraison**, un **workflow de
validation bloquant** et des **commandes** prêtes à l'emploi.

## Pourquoi ça marche

- `CLAUDE.md` à la racine est chargé automatiquement à chaque session et traité
  comme instructions faisant autorité (prioritaires sur la conversation).
- Il importe les règles via la syntaxe `@chemin` (récursive, profondeur max 5).
- Les `skills` sont déclenchées par l'agent au bon moment ; les `commands` sont
  invocables explicitement (`/validate`, `/new-feature`, `/commit`).
- En monorepo, chaque package a son `CLAUDE.md` chargé à la demande.

## Installation via Packmind (recommandée)

Nécessite [packmind-cli](https://www.packmind.com) installé et authentifié.

```sh
# 1. Installer le harness dans le projet courant
packmind install @global/ia-harness

# 2. Dans Claude Code, lancer la skill de configuration
# /install-harness
```

La skill `/install-harness` détecte automatiquement la stack technique
(Node.js, Python, Java, Go, Rust, C++/CMake…), inspecte la CI existante
pour extraire les commandes réelles, puis remplace tous les placeholders.
Elle pose une seule question : le préfixe JIRA.

## Installation manuelle

1. Copier le contenu de ce dossier à la **racine du dépôt**.
2. Renseigner les marqueurs `🔧 À COMPLÉTER` et les `{{TOKENS}}`
   (nom de projet, préfixes JIRA, commandes par langage…).
3. Pour chaque package, copier `packages/_TEMPLATE_PACKAGE/CLAUDE.md` dans le
   package et le compléter (langage, mapping hexagonal, commandes lint/build/test/e2e).
4. Activer le hook de commit :
   ```sh
   git config core.hooksPath scripts/hooks
   chmod +x scripts/hooks/commit-msg
   ```
5. Vérifier le chargement côté agent : lancer `/memory` dans Claude Code pour
   voir les fichiers d'instructions chargés et leur ordre.

## Arborescence

```
.
├── CLAUDE.md                                  # racine, auto-chargé, orchestrateur
├── .claude/
│   ├── rules/                                 # règles non négociables (importées)
│   │   ├── tdd.md
│   │   ├── hexagonal-architecture.md
│   │   ├── mitosis.md
│   │   ├── clean-code.md
│   │   └── test-coverage.md
│   ├── standards/                             # contrat de livraison
│   │   ├── commit-convention.md
│   │   └── definition-of-done.md
│   ├── skills/
│   │   └── validation-workflow/SKILL.md       # pipeline lint→build→test→e2e
│   └── commands/
│       ├── validate.md                        # /validate
│       ├── new-feature.md                     # /new-feature
│       └── commit.md                          # /commit
├── scripts/hooks/commit-msg                   # hook Conventional Commits + JIRA
└── packages/
    └── _TEMPLATE_PACKAGE/CLAUDE.md            # gabarit de contexte par package
```

## Portabilité vers d'autres agents

Le contenu des règles est du markdown standard et reste valable pour d'autres
outils. Pour les porter : pointer un import unique depuis le format attendu par
l'outil (`AGENTS.md`, `.cursor/rules`, `GEMINI.md`…) vers ce cœur partagé, afin
de conserver une **source de vérité unique**. (Hors périmètre actuel : cible Claude.)

## Ce que l'agent doit toujours faire

- Ne prendre **aucune** décision structurante sans validation préalable.
- Travailler en **TDD**, par petits incréments.
- Respecter l'**architecture hexagonale** et **diviser** les modules trop chargés.
- Maintenir le **domaine à 100 %** de couverture.
- Finir par `/validate` vert, puis un `/commit` conforme.
