# Test Coverage — Layer-Differentiated Policy

Enforce 100 % coverage on the domain core; apply targeted coverage on adapters.

## Rules

* Maintain 100 % line and branch coverage on all domain (business logic) code
* Treat uncovered domain code as a TDD violation — add the missing test before continuing
* Never lower the domain coverage threshold to make CI pass — fix the code instead
* Never exclude domain code from coverage measurement to bypass the rule
* Cover adapter logic with targeted tests focused on the adaptation logic itself
* Write assertions that verify behaviour, not just execution — coverage without assertions is misleading
* Cover branches and edge cases, not only the happy path
