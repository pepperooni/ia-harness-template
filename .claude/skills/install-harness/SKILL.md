---
name: install-harness
description: >
  Installe le harness Claude Code dans le projet courant : scanne la stack
  technique et les fichiers CI, génère CLAUDE.md et le hook git, résout les
  placeholders. À déclencher après `packmind install @global/ia-harness`.
---

# Skill — Installation du harness IA

Tu dois installer et configurer le harness Claude Code dans le projet courant.
Suis les étapes dans l'ordre. Annonce chaque étape avant de l'exécuter.

---

## Étape 1 — Détecter la stack

Scanne la racine du projet courant pour trouver les fichiers manifeste :

```
package.json, pnpm-lock.yaml, yarn.lock
pyproject.toml, setup.py, setup.cfg
pom.xml
build.gradle, build.gradle.kts
go.mod
Cargo.toml
CMakeLists.txt
```

Annonce la stack détectée. Si plusieurs manifestes coexistent (monorepo), liste
chaque package et sa stack.

---

## Étape 2 — Inspecter la CI (priorité absolue)

Cherche des fichiers CI dans le projet :

- `.github/workflows/*.yml` — steps GitHub Actions
- `.gitlab-ci.yml` — jobs GitLab CI
- `Jenkinsfile` — stages Jenkins
- `.circleci/config.yml` — jobs CircleCI
- `Makefile` — targets

Pour chaque fichier trouvé, extrais les commandes des jobs/steps dont le nom
contient : `lint`, `format`, `build`, `test`, `e2e`, `integration`, `quality`, `check`.

- GitHub Actions → champ `run:` des steps
- GitLab CI → champ `script:` des jobs
- Makefile → contenu de la recette de la target

Si plusieurs steps couvrent le même CMD_, concaténer avec ` && `.
Si la CI ne couvre pas un CMD_, fallback sur l'étape 3.
Signale pour chaque commande si elle vient de la **CI** ou du **mapping**.

---

## Étape 3 — Résoudre les commandes (fallback si CI incomplète)

| Stack détectée | CMD_LINT | CMD_BUILD | CMD_TEST | CMD_E2E |
|---|---|---|---|---|
| Node.js + pnpm-lock.yaml | `pnpm lint` | `pnpm build` | `pnpm test` | `pnpm test:e2e` |
| Node.js + yarn.lock | `yarn lint` | `yarn build` | `yarn test` | `yarn test:e2e` |
| Node.js (npm, défaut) | `npm run lint` | `npm run build` | `npm test` | `npm run test:e2e` |
| Python (pyproject.toml / setup.py) | `ruff check .` | *(non applicable)* | `pytest` | `pytest -m e2e` |
| Java Maven (pom.xml) | `mvn checkstyle:check` | `mvn compile` | `mvn test` | `mvn verify -Pintegration` |
| Java Gradle (build.gradle) | `./gradlew checkstyleMain` | `./gradlew compileJava` | `./gradlew test` | `./gradlew integrationTest` |
| Go (go.mod) | `golangci-lint run` | `go build ./...` | `go test ./...` | `go test -tags=e2e ./...` |
| Rust (Cargo.toml) | `cargo clippy` | `cargo build` | `cargo test` | `cargo test --features e2e` |
| C++/CMake (CMakeLists.txt) | `cmake --build . --target lint` | `cmake --build .` | `ctest` | `ctest -L e2e` |

Si une commande ne peut pas être déterminée, laisse `# TODO: à compléter` et signale-le.

---

## Étape 3bis — Détecter les dépendances d'infrastructure (conteneurs / stack)

Certains projets ne peuvent **pas** builder, tester (intégration) ou jouer leurs
e2e sans monter au préalable une stack d'infrastructure (base de données, broker,
conteneur applicatif…). Scanne largement les signaux suivants et **annonce**
chacun détecté. Les patterns sont alignés sur la catégorie **Docker** de
`.claude/skills/packmind-onboard/references/ci-local-workflow-parity.md`.

| Signal | Détection | Implication |
|---|---|---|
| `docker-compose.yml` / `.yaml`, `compose.yml` / `.yaml` (racine + sous-dossiers évidents) | présence fichier | stack multi-services → UP/DOWN via `docker compose` |
| `Dockerfile` (sans compose) | présence fichier | image applicative ; n'implique un UP/DOWN que si un test a besoin du conteneur lancé |
| Bloc `services:` dans la CI | grep `services:` dans `.github/workflows/*.yml`, `.gitlab-ci.yml` | services requis pendant `test` → la stack locale doit les reproduire |
| Testcontainers | grep `testcontainers` / `@testcontainers` / `org.testcontainers` dans les manifestes (package.json, pom.xml, build.gradle, go.mod, Cargo.toml, pyproject.toml) | les tests gèrent eux-mêmes les conteneurs ; **prérequis = démon Docker actif** (pas de UP/DOWN explicite) |
| `.devcontainer/devcontainer.json` | présence fichier | environnement de dev conteneurisé ; informatif |
| Target `up` / `services` / `infra` / `db` / `stack` dans `Makefile`, `Taskfile.yml`, `Justfile`, ou script npm `services:up`… | grep targets/scripts | **source prioritaire** des commandes UP/DOWN (même principe CI-first qu'à l'étape 2) |

### Déterminer `NEEDS_SERVICES_FOR`

Pour chaque signal, détermine quelle(s) étape(s) du pipeline le requièrent
(`build` / `test` / `e2e`). Heuristique par défaut :

- compose / `services:` CI / Testcontainers → **test + e2e** (build généralement natif) ;
- `Dockerfile` applicatif utilisé par le build → **build**.

> **Posture harness** : ne pas trancher une hypothèse structurante en silence.
> Si le rattachement étape↔stack est ambigu, **demander** à l'utilisateur quelles
> étapes nécessitent la stack avant de figer les placeholders.

### Résoudre `CMD_SERVICES_UP` / `CMD_SERVICES_DOWN`

Même logique de priorité que les `CMD_*` (commande projet explicite d'abord,
fallback générique ensuite) :

| Cas détecté | CMD_SERVICES_UP | CMD_SERVICES_DOWN |
|---|---|---|
| Target/script projet (`make up`, `task services`, `pnpm services:up`…) | la commande projet | sa contrepartie (`make down`…) |
| compose présent, pas de target | `docker compose -f <fichier> up -d --wait` | `docker compose -f <fichier> down -v` |
| Testcontainers seul | `# prérequis : démon Docker actif (vérifier avec docker info)` | `# n/a — géré par les tests` |
| Dockerfile seul, requis au build | commande `docker build` / `docker run` documentée | `# n/a` ou `docker rm` selon le cas |
| Aucun signal | `none` | `none` |

Si une commande est indéterminable, laisse `# TODO: à compléter` et signale-le
(cohérent avec l'étape 3).

---

## Étape 4 — Extraire le nom du projet

Lis le nom depuis le manifeste principal :

- `package.json` → `name`
- `pyproject.toml` → `[project].name`
- `pom.xml` → `<artifactId>`
- `build.gradle` → `rootProject.name` ou `archivesBaseName`
- `go.mod` → première ligne `module`
- `Cargo.toml` → `[package].name`
- `CMakeLists.txt` → premier argument de `project(...)`

---

## Étape 5 — Demander le préfixe JIRA

> Quel est le **préfixe JIRA** de ce projet ? (ex. `ABC`, `PAY`, `CORE`)
> Répondre `NONE` si ce projet n'utilise pas JIRA.

Attends la réponse.

---

## Étape 6 — Générer `CLAUDE.md` à la racine

Crée le fichier `CLAUDE.md` à la racine du projet avec le contenu ci-dessous.
Remplace `<NOM_DU_PROJET>` par le nom extrait à l'étape 4.

````markdown
# <NOM_DU_PROJET> — Contexte agent

> Fichier mémoire racine, chargé automatiquement par Claude Code à chaque session
> et traité comme **instructions faisant autorité**. Garder ce fichier court
> (< 200 lignes) : le détail vit dans les fichiers importés ci-dessous.

## Carte du monorepo

Avant toute action, situer le travail dans l'arborescence. Ne jamais placer de
logique métier hors de la couche `domain` (voir règle architecture hexagonale).

```
packages/
  <!-- 🔧 À COMPLÉTER : lister chaque package et sa responsabilité, ex. -->
  <!-- core-domain/      → règles métier pures, zéro dépendance technique     -->
  <!-- api/              → adaptateur entrant HTTP                            -->
  <!-- persistence/      → adaptateur sortant base de données                 -->
  <!-- ui-components/    → bibliothèque de composants                         -->
```

Chaque package possède son propre `CLAUDE.md` (contexte local + commandes
spécifiques au langage). Claude le charge à la demande en travaillant dans le sous-arbre.

## Règles non négociables

Ces règles s'appliquent à **tous** les projets et priment sur toute préférence
ponctuelle exprimée en conversation. En cas de conflit, demander avant de dévier.

@.claude/rules/packmind/tdd-test-driven-development.md
@.claude/rules/packmind/hexagonal-architecture-ports-adapters.md
@.claude/rules/packmind/mitosis-continuous-module-division.md
@.claude/rules/packmind/clean-code.md
@.claude/rules/packmind/test-coverage-layer-differentiated-policy.md
@.claude/rules/packmind/observability-structured-logs-and-traces.md

## Standards de livraison

@.claude/rules/packmind/definition-of-done.md
@.claude/rules/packmind/commit-convention-conventional-commits-jira.md

## Workflow de validation

Toute tâche de développement n'est considérée terminée qu'après passage **vert**
du pipeline de validation : `code quality → build → test → e2e`.
Détails et ordre d'exécution : voir la skill `validation-workflow`
(`.claude/skills/validation-workflow/SKILL.md`).

Commandes disponibles :
- `/validate`    — exécute le pipeline complet et bloque si une étape échoue
- `/new-feature` — démarre une fonctionnalité en boucle TDD + hexagonale
- `/commit`      — produit un commit conforme (Conventional Commits + clé JIRA)

## Posture de l'agent

- **Ne prendre aucune hypothèse structurante.** Si une décision touche
  l'architecture, le contrat d'une interface, le schéma de données ou un choix
  de dépendance : poser la question avant d'agir.
- Travailler par **petits incréments testés** ; ne jamais empiler du code non
  couvert (voir TDD + couverture).
- Avant de déclarer une tâche finie : exécuter `/validate` et coller le résultat.
````

**Note sur les chemins** : Packmind installe les standards dans `.claude/rules/packmind/<slug>.md`.
Vérifie que ce répertoire contient bien les fichiers après `packmind install`, et ajuste
les chemins `@`-import si nécessaire.

---

## Étape 7 — Remplacer les placeholders dans `validation-workflow/SKILL.md`

Dans `.claude/skills/validation-workflow/SKILL.md`, remplace :

- `{{CMD_LINT}}` → commande résolue aux étapes 2/3
- `{{CMD_BUILD}}` → commande résolue
- `{{CMD_TEST}}` → commande résolue
- `{{CMD_E2E}}` → commande résolue
- `{{CMD_SERVICES_UP}}` → commande de montage de la stack résolue à l'étape 3bis (ou `none`)
- `{{CMD_SERVICES_DOWN}}` → commande de démontage résolue à l'étape 3bis (ou `none`)
- `{{NEEDS_SERVICES_FOR}}` → étapes nécessitant la stack (ex. `test + e2e`) déterminées à l'étape 3bis

Supprime aussi les commentaires `<!-- 🔧 À COMPLÉTER … -->` autour de chaque commande.

Si aucun signal d'infrastructure n'a été détecté (étape 3bis), résous
`{{CMD_SERVICES_UP}}` et `{{CMD_SERVICES_DOWN}}` à `none` : l'étape 0 et le
teardown du pipeline seront alors neutralisés (voir `validation-workflow/SKILL.md`).

---

## Étape 8 — Générer `scripts/hooks/commit-msg`

Crée le répertoire `scripts/hooks/` s'il n'existe pas.
Crée le fichier `scripts/hooks/commit-msg` avec le contenu ci-dessous.
Si le préfixe JIRA est `NONE`, simplifie le grep (voir note en bas).

```sh
#!/usr/bin/env sh
# Hook commit-msg — valide la convention de commit du projet.
MSG_FILE="$1"
SUBJECT="$(sed -n '1p' "$MSG_FILE")"
BODY="$(sed -n '3,$p' "$MSG_FILE" | grep -v '^#' | tr -d '[:space:]')"
JIRA_PREFIXES="<JIRA_PREFIX>"
TYPES="feat|fix|refactor|test|docs|chore|build|ci|perf|style"
fail() { echo "✖ commit-msg: $1" >&2; exit 1; }
if ! printf '%s' "$SUBJECT" | grep -Eq "^($TYPES)(\([a-z0-9._-]+\))?: .+\[(${JIRA_PREFIXES})-[0-9]+\]$"; then
  fail "format attendu : <type>(<scope>): <résumé> [PROJET-123]
       reçu : $SUBJECT"
fi
if [ "${#SUBJECT}" -gt 100 ]; then
  fail "première ligne trop longue (${#SUBJECT} > 100)."
fi
if [ -z "$BODY" ]; then
  fail "description fonctionnelle manquante."
fi
echo "✔ commit-msg: conforme"
exit 0
```

Remplace `<JIRA_PREFIX>` par le préfixe saisi à l'étape 5.
Si préfixe = `NONE` : remplace le grep par :
```sh
if ! printf '%s' "$SUBJECT" | grep -Eq "^($TYPES)(\([a-z0-9._-]+\))?: .+"; then
```
Et supprime la ligne `JIRA_PREFIXES`.

Puis rends le fichier exécutable :
```sh
chmod +x scripts/hooks/commit-msg
```

---

## Étape 9 — Générer `packages/_TEMPLATE_PACKAGE/CLAUDE.md`

Si `packages/` ne contient aucun sous-répertoire avec un `CLAUDE.md`, crée
`packages/_TEMPLATE_PACKAGE/CLAUDE.md` :

````markdown
# Package <NOM_PACKAGE> — contexte local

> Chargé à la demande par Claude Code quand on travaille dans ce sous-arbre.
> Les règles globales du dépôt (TDD, hexagonale, Mitosis, clean code, couverture)
> s'appliquent **en plus** de ce fichier — ne pas les redéfinir, seulement
> préciser ce qui est spécifique à ce package.

## Rôle du package
<!-- 🔧 À COMPLÉTER : responsabilité unique de ce package + couche hexagonale dominante. -->

## Langage & outillage
<!-- 🔧 À COMPLÉTER : langage, version, gestionnaire de paquets/build. -->

## Mapping hexagonal → arborescence
<!-- 🔧 À COMPLÉTER, ex.
domain/      → règles métier pures
ports/       → interfaces
adapters/    → implémentations techniques
-->

## Commandes locales
```
CMD_LINT          = <!-- 🔧 lint + format -->
CMD_BUILD         = <!-- 🔧 build -->
CMD_TEST          = <!-- 🔧 tests + couverture (domaine 100% lignes+branches) -->
CMD_E2E           = <!-- 🔧 e2e -->
CMD_SERVICES_UP   = <!-- 🔧 monter la stack (docker compose up -d) ou `none` -->
CMD_SERVICES_DOWN = <!-- 🔧 démonter la stack ou `none` -->
```

## Spécificités de test
<!-- 🔧 À COMPLÉTER : framework, convention de nommage. -->
````

Si des packages existent déjà, propose de copier ce gabarit pour ceux sans `CLAUDE.md`.

---

## Étape 10 — Configurer le hook git

```sh
git config core.hooksPath scripts/hooks
```

Si le projet n'est pas un dépôt git, signaler qu'il faudra exécuter cette
commande après `git init`.

---

## Étape 11 — Rapport de fin

| Élément | Valeur |
|---|---|
| Projet | *nom* |
| Stack détectée | *stack* |
| Source CMD_LINT | CI / mapping / TODO |
| CMD_LINT | *valeur* |
| CMD_BUILD | *valeur* |
| CMD_TEST | *valeur* |
| CMD_E2E | *valeur* |
| Infra détectée | compose / Dockerfile / services CI / Testcontainers / devcontainer / aucune |
| Étapes nécessitant la stack | *valeur de NEEDS_SERVICES_FOR* (ou aucune) |
| CMD_SERVICES_UP | *valeur* (source : target projet / compose / mapping / TODO) |
| CMD_SERVICES_DOWN | *valeur* |
| Préfixe JIRA | *valeur* |
| Hook git | configuré / non (git non initialisé) |

> Lancer `/validate` pour vérifier que le pipeline est opérationnel.
> Si une commande est marquée `# TODO`, la renseigner avant de lancer `/validate`.

> **Miroirs Packmind** : les fichiers `.packmind/commands/*` et `.packmind/standards/*`
> sont des copies gérées par Packmind. Si le harness est distribué via Packmind,
> régénérer ces miroirs par le flow Packmind plutôt que de les éditer à la main.
