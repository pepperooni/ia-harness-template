---
name: 'Clean Code'
alwaysApply: true
description: 'Enforce intent-revealing naming, small single-responsibility functions with ≤3 parameters, early returns over deep nesting, named constants, and removal of dead code and nonessential comments to improve readability and long-term maintainability.'
---

# Standard: Clean Code

Enforce intent-revealing naming, small single-responsibility functions with ≤3 parameters, early returns over deep nesting, named constants, and removal of dead code and nonessential comments to improve readability and long-term maintainability. :
* Keep functions small and focused on a single responsibility at one level of abstraction
* Limit function parameters to three; use a value object beyond that
* Maintain consistent style across the entire repository
* Name functions as action verbs; name booleans as questions
* Name variables and functions by intent — avoid abbreviations and generic names like "data", "tmp", "manager"
* Remove dead code and outdated comments — delete rather than comment out
* Return early to avoid deep nesting rather than wrapping logic in conditions
* Use named constants instead of magic numbers or strings
* Write comments only to explain WHY, never WHAT the code does

Full standard is available here for further request: [Clean Code](../../../.packmind/standards/clean-code.md)