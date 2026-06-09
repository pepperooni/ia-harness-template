---
name: 'Hexagonal Architecture — Ports & Adapters'
alwaysApply: true
description: 'Enforce Hexagonal Architecture (Ports & Adapters) by isolating business rules and use cases in a dependency-free domain with inward-pointing port interfaces and peripheral adapters that never import other adapters, ensuring independently testable core logic and preventing infrastructure coupling.'
---

# Standard: Hexagonal Architecture — Ports & Adapters

Enforce Hexagonal Architecture (Ports & Adapters) by isolating business rules and use cases in a dependency-free domain with inward-pointing port interfaces and peripheral adapters that never import other adapters, ensuring independently testable core logic and preventing infrastructure coupling. :
* Announce the layer (domain / port / adapter) before creating any new file
* Define ports (interfaces) in the domain to express what the domain needs from the outside
* Implement adapters at the periphery — never let an adapter import another adapter
* Keep the domain independently testable without any infrastructure (no I/O, no framework)
* Place all business rules and use cases in the domain layer with zero technical dependencies
* Point all dependencies inward: adapter → port → domain, never the reverse
* Signal and fix any technical import that leaks into the domain immediately

Full standard is available here for further request: [Hexagonal Architecture — Ports & Adapters](../../../.packmind/standards/hexagonal-architecture-ports-adapters.md)