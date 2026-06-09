# Packmind Commands Index

This file contains all available coding commands that can be used by AI agents (like Cursor, Claude Code, GitHub Copilot) to find and use proven patterns in coding tasks.

## Available Commands

- [Commit](commands/commit.md) : Prepare a commit message that follows the commit-convention format by verifying `/validate`, requiring an explicit JIRA key, and ensuring it passes the `commit-msg` hook to produce consistent, automatable commit history when committing changes in a JIRA-tracked project.
- [New feature](commands/new-feature.md) : Implement a requested feature via small-step TDD in a hexagonal architecture, proposing any new ports/schemas/dependencies before coding to preserve stable design and ensure high domain coverage and validated changes when delivering incremental functionality safely.
- [Validate](commands/validate.md) : Exécuter le workflow de validation de façon strictement séquentielle (code quality → build → test → e2e), en stoppant au premier échec pour le diagnostiquer et proposer une correction puis relancer depuis le début, afin de garantir un pipeline fiable avec une couverture domaine à 100 % avant de conclure VALIDÉ ou BLOQUÉ lors de toute livraison ou modification de code.


---

*This file was automatically generated from deployed command versions.*