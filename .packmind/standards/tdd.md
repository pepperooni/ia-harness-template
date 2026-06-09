# TDD — Test-Driven Development

Write tests before production code, following the Red → Green → Refactor cycle.

## Rules

* Write a failing test before writing any production code
* Write only the minimum code needed to make the test pass
* Refactor code and tests after each green cycle, keeping coverage constant
* Verify that each new test fails before implementing — a test that cannot fail is not a test
* Never modify a test to make it pass — fix the code or challenge the requirement
* Name each test as a sentence describing the expected behaviour
* Structure tests with Arrange / Act / Assert (one behaviour per test)
