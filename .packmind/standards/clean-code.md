# Clean Code

Write code for readability; name things by intent and keep functions focused.

## Rules

* Name variables and functions by intent — avoid abbreviations and generic names like "data", "tmp", "manager"
* Name functions as action verbs; name booleans as questions
* Keep functions small and focused on a single responsibility at one level of abstraction
* Limit function parameters to three; use a value object beyond that
* Return early to avoid deep nesting rather than wrapping logic in conditions
* Use named constants instead of magic numbers or strings
* Remove dead code and outdated comments — delete rather than comment out
* Write comments only to explain WHY, never WHAT the code does
* Maintain consistent style across the entire repository
