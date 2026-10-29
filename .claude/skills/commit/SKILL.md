---
name: commit
description: Create a git commit whose message follows the project template in templates/commit.md (Conventional Commits subject, optional body, branch, session id, datetime). Use when the user asks to commit, or runs /commit.
argument-hint: "[optional hint for the commit message]"
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git branch:*), Bash(git rev-parse:*), Bash(git commit:*), Bash(date:*), Read
---

# Commit using the project template

Session id for this run: `${CLAUDE_SESSION_ID}`

User hint (may be empty): $ARGUMENTS

## 1. Load the template

Read `templates/commit.md` at the repo root. It is the source of truth for the message format. If it disagrees with the summary below, follow the file. If it's missing, stop and tell the user.

Current template (summary):

```
<type>: <message>
<body>
branch: <branch>
session_id: <CLAUDE_SESSION_ID>
datetime: <datetime>
```

## 2. Gather context

Run these in parallel:

- `git status --short`
- `git diff --staged` and `git diff`
- `git rev-parse --abbrev-ref HEAD` (gives `<branch>`)
- `git log --oneline -10` (to match the existing style)
- `date +"%Y-%m-%dT%H:%M:%S%z"` (gives `<datetime>`, ISO 8601 with offset)

**Rule: commit only what is already staged.** Never run `git add`, `git rm`, `git commit -a` or `git commit <paths>`, and never stage or unstage anything yourself. Unstaged and untracked changes stay out of the commit, even if they look related.

If nothing is staged, tell the user and stop. Don't offer to stage files for them. Write the message from `git diff --staged` only. Use `git status` and `git diff` just to point out to the user what's left out.

## 3. Fill the fields

- `<type>`: a Conventional Commits type that fits the change: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore` or `revert`. You can add a scope (`feat(skills): ...`). Use `!` or a `BREAKING CHANGE:` line for breaking changes.
- `<message>`: a short imperative summary, lowercase, with no trailing period. Keep the whole subject line under about 72 characters.
- `<body>`: optional. A `- ` bullet list of the changes made. Leave the line out entirely when the subject already says enough.
- `<branch>`: the current branch from step 2.
- `<CLAUDE_SESSION_ID>`: the session id shown at the top of this skill. If it's empty or still reads literally as `${CLAUDE_SESSION_ID}`, write `unknown` and tell the user.
- `<datetime>`: the value from `date` in step 2.

## 4. Commit

Pass the message through a heredoc so the line breaks survive:

```bash
git commit -F - <<'EOF'
docs: add commit skill
- add .claude/skills/commit/SKILL.md
branch: main
session_id: 3ed81bb6-...
datetime: 2026-10-29T20:55:00-0300
EOF
```

Don't use `--no-verify` or `--amend` unless the user asks. If a hook fails, fix the problem and make a new commit. Don't retry with the hook skipped.

## 5. Report

Run `git log -1` and show the user the commit hash and message.
