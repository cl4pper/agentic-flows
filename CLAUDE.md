# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A documentation-only collection of reusable agentic workflows meant to be applied across other projects. There is no code, build, lint, or test tooling. The deliverables are Markdown files.

## Structure

- `workflows/<version>/` holds one workflow version (e.g. `workflows/v1.0.0/`). The directory name is a semver tag for the workflow definition.
- Each step of a workflow is its own file named `step_<ID>.md` (e.g. `step_A.md`). Step IDs are letters.

## Templates

`templates/` holds the formats the repo's files follow and is the source of truth for them:

- `templates/step.md`: sections every `step_<ID>.md` must have, in order
- `templates/commit.md`: commit message format used by `/commit`

Read the template before writing a step or a commit message. When a format changes, change it in the template, not here.

## Skills

- `.claude/skills/commit/`: `/commit` writes a commit message from `templates/commit.md` and commits **only already-staged changes**, after the user approves a commit plan. Never stage files on its behalf. Usage is documented in `README.md`; keep both in sync when the skill changes.
