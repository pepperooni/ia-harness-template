---
name: 'Definition of Done'
alwaysApply: true
description: 'Define task completion criteria using TDD, hexagonal architecture, 100% domain line/branch coverage, a green code-quality/build/test/e2e pipeline, Conventional Commits with JIRA keys, structured logging plus observability spans/metrics, module de-overloading (Mitosis), and pre-decision validation to ensure consistent quality and reliable delivery.'
---

# Standard: Definition of Done

Define task completion criteria using TDD, hexagonal architecture, 100% domain line/branch coverage, a green code-quality/build/test/e2e pipeline, Conventional Commits with JIRA keys, structured logging plus observability spans/metrics, module de-overloading (Mitosis), and pre-decision validation to ensure consistent quality and reliable delivery. :
* Add spans and metrics for every new use case when an observability framework is present
* Commit with Conventional Commits format, a JIRA key, and a functional description
* Emit structured logs at every significant functional boundary
* Implement every feature through the TDD cycle — no production code without a prior failing test
* Keep domain coverage at 100 % lines and branches after every cycle
* Obtain validation before any architectural, interface-contract, or dependency decision
* Pass the full validation pipeline green: code quality → build → test → e2e
* Respect hexagonal architecture: no technical detail in the domain, dependencies point inward
* Split overloaded modules (Mitosis) before declaring the task done

Full standard is available here for further request: [Definition of Done](../../../.packmind/standards/definition-of-done.md)