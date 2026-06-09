# Hexagonal Architecture — Ports & Adapters

Design all features around a pure domain core with ports and adapters on the outside.

## Rules

* Place all business rules and use cases in the domain layer with zero technical dependencies
* Define ports (interfaces) in the domain to express what the domain needs from the outside
* Implement adapters at the periphery — never let an adapter import another adapter
* Point all dependencies inward: adapter → port → domain, never the reverse
* Keep the domain independently testable without any infrastructure (no I/O, no framework)
* Announce the layer (domain / port / adapter) before creating any new file
* Signal and fix any technical import that leaks into the domain immediately
