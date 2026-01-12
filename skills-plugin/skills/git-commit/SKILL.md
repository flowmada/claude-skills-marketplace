---
name: git-commit
description: "Use when user asks to commit changes. Favor concision, stage files separately for review."
---
# Git Commit Workflow

## Rules

1. **Concision over grammar** - Keep commit messages short and direct
2. **Stage first, then commit** - Always `git add` as a separate step so user can review staged files before committing
3. **NEVER use `git add -A` or `git add .`** - Only stage files you actually modified in the current work session
4. **No non-code files** - Don't stage images, documentation, settings, or other files unrelated to the code changes

## Process

1. List the specific files you modified in this session
2. Run `git add <file1> <file2> ...` with explicit paths
3. Run `git status` to show what was staged
4. Ask: "Are the files staged correctly? Y/N"
5. If Y: Run `git commit` with concise message
6. If N: Ask "What's the problem?" and fix it
