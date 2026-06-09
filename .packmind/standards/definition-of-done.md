# Definition of Done

A task is complete only when all quality and delivery conditions are met.

## Rules

* Implement every feature through the TDD cycle — no production code without a prior failing test
* Respect hexagonal architecture: no technical detail in the domain, dependencies point inward
* Split overloaded modules (Mitosis) before declaring the task done
* Keep domain coverage at 100 % lines and branches after every cycle
* Pass the full validation pipeline green: code quality → build → test → e2e
* Commit with Conventional Commits format, a JIRA key, and a functional description
* Emit structured logs at every significant functional boundary
* Add spans and metrics for every new use case when an observability framework is present
* Obtain validation before any architectural, interface-contract, or dependency decision
