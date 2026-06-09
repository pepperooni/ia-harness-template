---
name: 'Test Coverage — Layer-Differentiated Policy'
alwaysApply: true
description: 'Enforce 100% line and branch test coverage for domain business logic and targeted behavior-focused tests for adapter adaptation logic to prevent untested core rules, catch edge cases, and keep CI quality gates meaningful.'
---

# Standard: Test Coverage — Layer-Differentiated Policy

Enforce 100% line and branch test coverage for domain business logic and targeted behavior-focused tests for adapter adaptation logic to prevent untested core rules, catch edge cases, and keep CI quality gates meaningful. :
* Cover adapter logic with targeted tests focused on the adaptation logic itself
* Cover branches and edge cases, not only the happy path
* Maintain 100 % line and branch coverage on all domain (business logic) code
* Never exclude domain code from coverage measurement to bypass the rule
* Never lower the domain coverage threshold to make CI pass — fix the code instead
* Treat uncovered domain code as a TDD violation — add the missing test before continuing
* Write assertions that verify behaviour, not just execution — coverage without assertions is misleading

Full standard is available here for further request: [Test Coverage — Layer-Differentiated Policy](../../../.packmind/standards/test-coverage-layer-differentiated-policy.md)