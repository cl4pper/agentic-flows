# Agentic Workflows
Agentic workflows that can be applied across various projects.

## Ways of work

### commit
Run `/commit` (optionally with a hint for the message, e.g. `/commit mention the new template`) to create a commit following the rules.

- **Stage your changes before running the command.**
- A commit plan (staged files and full message) is shown first; nothing is committed until you approve it.
- Git hooks are never skipped and commits are never amended unless asked.
- The message format comes from `templates/commit.md`.
