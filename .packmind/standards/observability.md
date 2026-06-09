# Observability — Structured Logs and Traces

Emit structured logs at functional boundaries and instrument use cases with spans and metrics.

## Rules

* Log at functional boundaries only: use-case entry, notable business decisions, errors, and long-process completion
* Include at minimum the fields `action` (past-tense business verb), `context` (key identifier), and `level` in every log
* Inject a single logger instance from the composition root — never instantiate a logger locally
* Never use console.log, print(), System.out.println, or any direct stdout/stderr write
* Never log passwords, tokens, or personal data without masking
* Use structured key-value fields instead of string interpolation in log messages
* Add a span per new use case when an observability framework (OpenTelemetry, Datadog…) is present
* Attach business identifiers (orderId, userId…) as span attributes — no sensitive data
* Define Logger and Tracer as outgoing ports in the domain — never import an SDK directly from domain code
