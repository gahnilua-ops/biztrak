# Codex Agent Instructions

## Git Workflow
- Before pushing, always run `git branch` to check the current branch name.
- Before pushing, always run `git remote -v` to verify the remote is configured.
- If no remote named `origin` exists, add it:
  `git remote add origin https://github.com/gahnilua-ops/biztrak.git`
- Push using the current branch name, not a hardcoded one:
  `git push origin HEAD`
- Do NOT assume the branch is called `main` — always detect it dynamically.
- Do NOT open a pull request unless explicitly asked.

## Commit Message Format
- Use present tense: "Fix bug in login flow" not "Fixed bug"
- Keep the first line under 72 characters

## General Rules
- Run existing tests before pushing if a test command is available.
- Do not push if tests fail — report the failure instead.
- Ask for clarification if the task is ambiguous before making changes.
