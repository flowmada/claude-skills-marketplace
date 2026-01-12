# claude-skills-marketplace

Private marketplace for personal Claude Code skills.

> **Getting started?** See [SETUP.md](SETUP.md) for installation instructions.

---

## Skills

### git-commit

Streamlined git commit workflow with explicit file staging.

**Trigger:** "commit", "stage changes", "save to git"

**Features:**
- Concision over grammar in commit messages
- Two-step workflow: stage first, then commit (allows review)
- Never uses `git add -A` or `git add .` - only stages explicitly modified files
- Excludes non-code files (images, documentation, settings)

**Process:**
1. Lists specific files modified in the session
2. Stages with explicit paths
3. Shows staged files for review
4. Asks for confirmation before committing

---

*More skills coming soon. Use `publish <skill-name>` to add skills from `~/.claude/skills/`.*
