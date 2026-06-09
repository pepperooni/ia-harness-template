---
name: 'TDD — Test-Driven Development'
alwaysApply: true
description: 'Adopt Test-Driven Development by writing a failing test first, implementing minimal code to pass, then refactoring in a Red→Green→Refactor loop with Arrange/Act/Assert and behavior-sentence test names to improve design feedback, prevent regressions, and maintain reliable coverage.'
---

# Standard: TDD — Test-Driven Development

Adopt Test-Driven Development by writing a failing test first, implementing minimal code to pass, then refactoring in a Red→Green→Refactor loop with Arrange/Act/Assert and behavior-sentence test names to improve design feedback, prevent regressions, and maintain reliable coverage. :
* Name each test as a sentence describing the expected behaviour
* Never modify a test to make it pass — fix the code or challenge the requirement
* Refactor code and tests after each green cycle, keeping coverage constant
* Structure tests with Arrange / Act / Assert (one behaviour per test)
* Verify that each new test fails before implementing — a test that cannot fail is not a test
* Write a failing test before writing any production code
* Write only the minimum code needed to make the test pass

Full standard is available here for further request: [TDD — Test-Driven Development](../../../.packmind/standards/tdd-test-driven-development.md)