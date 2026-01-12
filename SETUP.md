# Setup Guide

## Installation

### 1. Register the marketplace

```
/plugin marketplace add flowmada/claude-skills-marketplace
```

### 2. Install plugins

```
/plugin install git-commit@claude-skills-marketplace --scope user
/plugin install test-planning@claude-skills-marketplace --scope user
```

### 3. Update plugins

```
/plugin marketplace update
/plugin update git-commit@claude-skills-marketplace
/plugin update test-planning@claude-skills-marketplace
```

## Repository Structure

```
claude-skills-marketplace/
├── .claude-plugin/
│   └── marketplace.json
├── git-commit/
│   ├── .claude-plugin/
│   │   └── plugin.json      # v1.0.0
│   └── skills/
│       └── git-commit/
├── test-planning/
│   ├── .claude-plugin/
│   │   └── plugin.json      # v1.0.0
│   └── skills/
│       ├── test-plan-01-overview/
│       ├── test-plan-02-analysis/
│       └── test-plan-03-specific-tests/
├── README.md
└── SETUP.md
```

## Private Repo Access

To use this marketplace on another machine:
1. Add your GitHub account as a collaborator to this repo
2. Authenticate with GitHub on the target machine
3. Run the installation commands above
