# software-engineering-plugin

Software Engineering Plugin for Claude Code

## Overview

This is a Claude Code plugin that provides development tools and workflows.

## Installation

To install this plugin in Claude Code:

```bash
# Add this repository as a marketplace
/plugin marketplace add mmarschall/software-engineering-plugin

# Install the plugin
/plugin install software-engineering-plugin@software-engineering-marketplace
```

Or install locally for development:

```bash
# Add current directory as a local marketplace
/plugin marketplace add .

# Install the plugin from the local marketplace
/plugin install software-engineering-plugin@.
```

## Structure

This plugin follows the standard Claude Code plugin structure:

```
software-engineering-plugin/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest (required)
├── commands/                # Slash commands
│   └── example.md
├── agents/                  # Specialized subagents
│   └── README.md
├── skills/                  # Reusable capabilities
│   └── example-skill/
│       └── SKILL.md
├── hooks/                   # Event handlers
│   └── README.md
├── scripts/                 # Executable utilities
│   └── README.md
├── LICENSE
└── README.md
```

## Components

### Commands

Slash commands that users can invoke with `/command-name`. Commands are defined as markdown files in the `commands/` directory.

### Agents

Specialized subagents for handling specific types of tasks. Defined as markdown files in the `agents/` directory.

### Skills

Reusable capabilities that Claude can use during conversations. Each skill is in its own subdirectory with a `SKILL.md` file.

### Hooks

Event handlers that execute scripts in response to Claude Code events. Configuration files go in the `hooks/` directory.

### Scripts

Executable utilities referenced by hooks and other plugin components.

## Development

To develop this plugin:

1. Clone the repository
2. Make your changes
3. Test locally with `claude plugin install .`
4. Submit a pull request

## License

MIT License - see LICENSE file for details
