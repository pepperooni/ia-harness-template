# Commit Convention — Conventional Commits + JIRA

All commits must follow Conventional Commits, include a JIRA key, and carry a functional description.

## Rules

* Format the subject line as: `<type>(<scope>): <imperative summary> [PROJ-123]`
* Use only allowed types: feat, fix, refactor, test, docs, chore, build, ci, perf, style
* Keep the subject line to 72 characters or fewer, no trailing period, no leading capital
* Always include a JIRA key in the subject — ask for it if missing, never invent one
* Write a functional body describing what changes for the user and why — not a technical diff summary
* Keep body lines to 100 characters or fewer
* Ensure the commit passes the commit-msg hook before pushing
