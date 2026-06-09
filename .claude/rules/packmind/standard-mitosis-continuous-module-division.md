---
name: 'Mitosis — Continuous Module Division'
alwaysApply: true
description: 'Split overloaded modules into cohesive, independently testable units during the Refactor phase of TDD (with green tests and constant coverage) to improve maintainability and prevent changes from impacting unrelated responsibilities.'
---

# Standard: Mitosis — Continuous Module Division

Split overloaded modules into cohesive, independently testable units during the Refactor phase of TDD (with green tests and constant coverage) to improve maintainability and prevent changes from impacting unrelated responsibilities. :
* Avoid fragmenting cohesive logic: a split must increase cohesion, not just reduce file size
* Ensure each resulting unit is independently testable after a split
* Perform splits during the Refactor phase of TDD, with tests green and coverage constant
* Propose a split before adding code to an overloaded unit — do not wait for a cleanup sprint
* Split a module as soon as its name contains "and", "manager", "utils", "helper", or "misc"
* Split when a functional change forces modifications to unrelated parts of the module
* Split when two groups of functions share no common state within the same file

Full standard is available here for further request: [Mitosis — Continuous Module Division](../../../.packmind/standards/mitosis-continuous-module-division.md)