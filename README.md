# claude-skills-marketplace

Private marketplace for personal Claude Code skills.

## Usage

### Register the marketplace

```bash
/plugin marketplace add flowmada/claude-skills-marketplace
```

### Install skills

```bash
/plugin install flowmada-skills@claude-skills-marketplace --scope user
```

### Update skills

```bash
/plugin marketplace update
/plugin update flowmada-skills@claude-skills-marketplace
```

## Structure

```
claude-skills-marketplace/
├── .claude-plugin/
│   └── marketplace.json    # Marketplace catalog
├── skills-plugin/
│   ├── .claude-plugin/
│   │   └── plugin.json     # Plugin manifest
│   ├── skills/             # Published skills
│   └── README.md
└── README.md
```

## Publishing Skills

Skills are published from `~/.claude/skills/` using the `publish-skill` skill:

```
"publish ios-unit-testing"
```

This syncs the skill folder to this marketplace and pushes to GitHub.
