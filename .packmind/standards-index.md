# Packmind Standards Index

This standards index contains all available coding standards that can be used by AI agents (like Cursor, Claude Code, GitHub Copilot) to find and apply proven practices in coding tasks.

## Available Standards

- [Clean Code](./standards/clean-code.md) : Enforce intent-revealing naming, small single-responsibility functions with ≤3 parameters, early returns over deep nesting, named constants, and removal of dead code and nonessential comments to improve readability and long-term maintainability.
- [Commit Convention — Conventional Commits + JIRA](./standards/commit-convention-conventional-commits-jira.md) : Enforce Conventional Commits with a JIRA key using the `<type>(<scope>): <imperative summary> [PROJ-123]` format (allowed types only, 72-char subject, functional body with 100-char lines) to improve traceability, user-impact clarity, and commit-msg hook compliance.
- [Definition of Done](./standards/definition-of-done.md) : Define task completion criteria using TDD, hexagonal architecture, 100% domain line/branch coverage, a green code-quality/build/test/e2e pipeline, Conventional Commits with JIRA keys, structured logging plus observability spans/metrics, module de-overloading (Mitosis), and pre-decision validation to ensure consistent quality and reliable delivery.
- [Hexagonal Architecture — Ports & Adapters](./standards/hexagonal-architecture-ports-adapters.md) : Enforce Hexagonal Architecture (Ports & Adapters) by isolating business rules and use cases in a dependency-free domain with inward-pointing port interfaces and peripheral adapters that never import other adapters, ensuring independently testable core logic and preventing infrastructure coupling.
- [Mitosis — Continuous Module Division](./standards/mitosis-continuous-module-division.md) : Split overloaded modules into cohesive, independently testable units during the Refactor phase of TDD (with green tests and constant coverage) to improve maintainability and prevent changes from impacting unrelated responsibilities.
- [Observability — Structured Logs and Traces](./standards/observability-structured-logs-and-traces.md) : Enforce structured boundary logging and use-case tracing via injected Logger/Tracer ports (no console/stdout), required `action`/`context`/`level` fields, key-value messages, sensitive-data masking, and OpenTelemetry/Datadog spans with business-ID attributes to improve diagnosability and preserve domain isolation.
- [TDD — Test-Driven Development](./standards/tdd-test-driven-development.md) : Adopt Test-Driven Development by writing a failing test first, implementing minimal code to pass, then refactoring in a Red→Green→Refactor loop with Arrange/Act/Assert and behavior-sentence test names to improve design feedback, prevent regressions, and maintain reliable coverage.
- [Test Coverage — Layer-Differentiated Policy](./standards/test-coverage-layer-differentiated-policy.md) : Enforce 100% line and branch test coverage for domain business logic and targeted behavior-focused tests for adapter adaptation logic to prevent untested core rules, catch edge cases, and keep CI quality gates meaningful.


---

*This standards index was automatically generated from deployed standard versions.*