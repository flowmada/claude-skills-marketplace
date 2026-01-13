# claude-skills-flowmada

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

## test-planning

Three-phase workflow for creating thorough test plans before writing tests.

**Skills included:**
- `test-plan-01-overview` - Creates foundation: What It Does, Model Structure, Out of Scope
- `test-plan-02-analysis` - Analyzes each testable part (simple explanation or formal artifacts)
- `test-plan-03-specific-tests` - Defines exact test cases with Arrange/Act/Assert

**Trigger:** "create a test plan", "write tests for [code]"

**Process:**
1. Overview phase → user approval
2. Analysis phase (one part at a time) → user approval
3. Specific tests phase (one part at a time) → user approval
4. Tests can then be written mechanically from the specification

---

*Use `publish <skill-name>` to add skills from `~/.claude/skills/`.*
