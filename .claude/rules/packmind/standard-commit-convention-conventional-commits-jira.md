---
name: 'Commit Convention — Conventional Commits + JIRA'
alwaysApply: true
description: 'Enforce Conventional Commits with a JIRA key using the `<type>(<scope>): <imperative summary> [PROJ-123]` format (allowed types only, 72-char subject, functional body with 100-char lines) to improve traceability, user-impact clarity, and commit-msg hook compliance.'
---

# Standard: Commit Convention — Conventional Commits + JIRA

Enforce Conventional Commits with a JIRA key using the `<type>(<scope>): <imperative summary> [PROJ-123]` format (allowed types only, 72-char subject, functional body with 100-char lines) to improve traceability, user-impact clarity, and commit-msg hook compliance. :
* Always include a JIRA key in the subject — ask for it if missing, never invent one
* Ensure the commit passes the commit-msg hook before pushing
* Format the subject line as: `<type>(<scope>): <imperative summary> [PROJ-123]`
* Keep body lines to 100 characters or fewer
* Keep the subject line to 72 characters or fewer, no trailing period, no leading capital
* Use only allowed types: feat, fix, refactor, test, docs, chore, build, ci, perf, style
* Write a functional body describing what changes for the user and why — not a technical diff summary

Full standard is available here for further request: [Commit Convention — Conventional Commits + JIRA](../../../.packmind/standards/commit-convention-conventional-commits-jira.md)