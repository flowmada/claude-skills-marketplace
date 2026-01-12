# Setup Guide

## Installation

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

## Repository Structure

```
claude-skills-marketplace/
├── .claude-plugin/
│   └── marketplace.json    # Marketplace catalog
├── skills-plugin/
│   ├── .claude-plugin/
│   │   └── plugin.json     # Plugin manifest
│   ├── skills/             # Published skills
│   └── README.md
├── README.md               # Skills documentation
└── SETUP.md                # This file
```

## Publishing Skills

Skills are published from `~/.claude/skills/` using the `publish-skill` skill:

```
"publish ios-unit-testing"
```

This syncs the skill folder to this marketplace and pushes to GitHub.

## Private Repo Access

To use this marketplace on another machine:
1. Add your GitHub account as a collaborator to this repo
2. Authenticate with GitHub on the target machine
3. Run the installation commands above
