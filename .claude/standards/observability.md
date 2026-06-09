# Règle — Observabilité (logs structurés + traces/métriques)

Toute fonctionnalité livrée doit être **observable en production**. L'observabilité
n'est pas un ajout cosmétique : c'est la preuve que le code fait ce qu'il prétend
faire et que les équipes peuvent diagnostiquer sans déployer un debugger.

## Principe

- Les logs existent pour répondre à la question : *« Que s'est-il passé
  fonctionnellement ? »* — pas *« Où en est l'exécution ? »*.
- Les traces et métriques existent pour répondre à : *« Est-ce que ça marche
  correctement à l'échelle ? »*.
- Sans ces deux dimensions, un service en production est une boîte noire.

## Logs structurés — quand et comment

### Quand loguer

Loguer **aux frontières fonctionnelles significatives** :

| Moment | Niveau | Exemple |
|--------|--------|---------|
| Entrée d'un cas d'usage avec ses paramètres clés | `info` | `order.placed { orderId, customerId, amount }` |
| Décision métier notable (branchement, règle appliquée) | `info` | `discount.applied { code, percent, orderId }` |
| Événement d'erreur fonctionnelle (règle violée, état invalide) | `warn` / `error` | `payment.rejected { reason, orderId }` |
| Sortie d'un processus long (job, batch, saga) | `info` | `import.completed { processed, failed, duration_ms }` |

Ne pas loguer à chaque ligne, dans chaque getter/setter, ni à l'entrée de
fonctions internes. La verbosité nuit à la lisibilité des logs.

### Format d'un log structuré

Chaque log doit porter au minimum :

```
action   — verbe métier à l'infinitif passé (ex. "order.placed", "payment.failed")
context  — objet ou identifiant concerné (ex. orderId, userId)
level    — info | warn | error selon la gravité fonctionnelle
```

Champs optionnels utiles : `duration_ms`, `reason`, `correlationId`.

```json
{ "action": "order.placed", "orderId": "abc-123", "customerId": "u-456", "amount": 99.90 }
{ "action": "payment.failed", "orderId": "abc-123", "reason": "insufficient_funds" }
```

### Logger unique à la maille projet

Un **seul logger** est instancié et configuré pour l'ensemble du projet, à la
composition root. Tout le code qui a besoin de loguer reçoit ce logger par
injection — il ne l'instancie jamais lui-même.

```
composition root
  └── configure logger (format JSON, niveau, destination)
        └── injecter dans tous les adapters via DI / constructeur
```

- Ne jamais créer un logger local (`new Logger()`, `logging.getLogger(__name__)`
  appelé hors composition root, `LoggerFactory.getLogger(MyClass.class)`
  inline dans une classe).
- Ne jamais passer par un singleton global statique accessible en dehors de la
  composition root.
- Le logger configuré est l'unique point de contrôle du format, du niveau et
  de la destination.

### Interdits absolus

- **`System.out.println` / `System.err.println`** (Java/Kotlin)
- **`console.log` / `console.error` / `console.warn`** (JS/TS)
- **`print()` / `pprint()`** (Python)
- **`fmt.Println` / `fmt.Printf`** utilisé à des fins de log (Go)
- Tout équivalent d'écriture directe sur stdout/stderr en lieu et place d'un log.

Ces appels contournent le logger, échappent à toute configuration (format,
niveau, destination) et polluent stdout en production. Ils sont interdits dans
**tout** fichier de l'application, y compris les scripts utilitaires.

Autres interdits :
- `logger.info("entering function")` — aucune valeur fonctionnelle.
- `logger.debug("done")` — informatif pour personne.
- Loguer des mots de passe, tokens, données personnelles sans masquage.
- Interpolation de chaîne non structurée : `"Order " + id + " placed"`.

## Instrumentation — si framework d'observabilité détecté

### Détection

Avant tout développement, vérifier la présence d'un framework d'observabilité :

```bash
grep -rE 'opentelemetry|@opentelemetry|otel|dd-trace|datadog|micrometer|prometheus' \
  package.json pyproject.toml pom.xml go.mod Cargo.toml 2>/dev/null
```

Si un résultat est trouvé → l'instrumentation est **obligatoire** pour toute
nouvelle fonctionnalité ou processus. Si rien n'est trouvé → suggérer l'ajout
d'un framework si le projet a une vocation de production, mais ne pas bloquer.

### Ce qui est obligatoire si framework présent

| Élément | Quand |
|---------|-------|
| **Span** | Un span par nouveau cas d'usage (use case) ou processus de fond (job, worker, saga) |
| **Attributs de span** | Identifiants métier clés (orderId, userId…), pas de données sensibles |
| **Counter** | Tout événement métier quantifiable (ex. `orders.created.total`) |
| **Histogram** | Toute opération dont la durée est significative (ex. `payment.processing.duration_ms`) |

Un span sans attributs métier est aussi inutile qu'un log sans contexte.

### Exemple (pseudo-code, indépendant du langage)

```
span = tracer.startSpan("order.place")
span.setAttribute("orderId", order.id)
span.setAttribute("customerId", order.customerId)
try:
    result = placeOrder(order)
    metrics.counter("orders.created").increment()
    span.setStatus(OK)
    return result
catch FunctionalError as e:
    span.setStatus(ERROR, e.message)
    metrics.counter("orders.rejected", reason=e.code).increment()
    raise
finally:
    span.end()
```

## Placement dans l'architecture hexagonale

`Logger` et `Tracer` sont des **ports sortants** : le domaine les connaît comme
des interfaces, jamais comme des SDKs concrets.

```
domain/
  ports/
    LoggerPort      ← interface définie par le domaine (optionnel si logs en adapter)
    TracerPort      ← interface définie par le domaine
adapters/
  observability/
    OtelTracerAdapter   ← implémente TracerPort via OpenTelemetry SDK
    PinoLoggerAdapter   ← implémente LoggerPort via Pino/Winston/…
```

**Règle absolue** : le domaine n'importe jamais `@opentelemetry/api`, `pino`,
`winston` ou tout autre SDK de log/trace directement. L'injection se fait
à la composition root.

Si les logs sont émis uniquement à la frontière du port entrant (adapter HTTP,
CLI, event consumer), un `LoggerPort` dans le domaine peut ne pas être nécessaire.

## Garde-fous pour l'agent

1. **Avant de démarrer une fonctionnalité** : exécuter le grep de détection et
   annoncer si l'instrumentation est obligatoire ou optionnelle.
2. **À chaque use case créé** : vérifier qu'un span et les métriques associées
   sont planifiés (si framework présent).
3. **À chaque log ajouté** : vérifier qu'il passe par le logger injecté, qu'il
   porte `action` + `context`, et qu'il n'est pas creux.
4. **Détecter et bloquer** tout `System.out`, `console.log`, `print()` ou
   équivalent : les signaler comme violation et les remplacer par le logger.
5. **Si un import OTEL ou logger SDK remonte dans le domaine** : signaler
   immédiatement et proposer l'extraction vers un port.
5. **En phase Refactor** : consolider les logs dupliqués et vérifier que les
   spans couvrent bien les chemins d'erreur.
