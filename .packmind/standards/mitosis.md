# Mitosis — Continuous Module Division

Split any module that carries more than one responsibility before adding more code to it.

## Rules

* Split a module as soon as its name contains "and", "manager", "utils", "helper", or "misc"
* Split when two groups of functions share no common state within the same file
* Split when a functional change forces modifications to unrelated parts of the module
* Perform splits during the Refactor phase of TDD, with tests green and coverage constant
* Ensure each resulting unit is independently testable after a split
* Propose a split before adding code to an overloaded unit — do not wait for a cleanup sprint
* Avoid fragmenting cohesive logic: a split must increase cohesion, not just reduce file size
