---
name: 'Observability — Structured Logs and Traces'
alwaysApply: true
description: 'Enforce structured boundary logging and use-case tracing via injected Logger/Tracer ports (no console/stdout), required `action`/`context`/`level` fields, key-value messages, sensitive-data masking, and OpenTelemetry/Datadog spans with business-ID attributes to improve diagnosability and preserve domain isolation.'
---

# Standard: Observability — Structured Logs and Traces

Enforce structured boundary logging and use-case tracing via injected Logger/Tracer ports (no console/stdout), required `action`/`context`/`level` fields, key-value messages, sensitive-data masking, and OpenTelemetry/Datadog spans with business-ID attributes to improve diagnosability and preserve domain isolation. :
* Add a span per new use case when an observability framework (OpenTelemetry, Datadog…) is present
* Attach business identifiers (orderId, userId…) as span attributes — no sensitive data
* Define Logger and Tracer as outgoing ports in the domain — never import an SDK directly from domain code
* Include at minimum the fields `action` (past-tense business verb), `context` (key identifier), and `level` in every log
* Inject a single logger instance from the composition root — never instantiate a logger locally
* Log at functional boundaries only: use-case entry, notable business decisions, errors, and long-process completion
* Never log passwords, tokens, or personal data without masking
* Never use console.log, print(), System.out.println, or any direct stdout/stderr write
* Use structured key-value fields instead of string interpolation in log messages

Full standard is available here for further request: [Observability — Structured Logs and Traces](../../../.packmind/standards/observability-structured-logs-and-traces.md)