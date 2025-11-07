# dev-forge-plugin

Development Forge Plugin for Claude Code

## Overview

This is a Claude Code plugin that provides development tools and workflows.

## Installation

To install this plugin in Claude Code:

```bash
claude-code plugin install https://github.com/mmarschall/dev-forge-plugin
```

Or install locally for development:

```bash
claude-code plugin install /path/to/dev-forge-plugin
```

## Structure

This plugin follows the standard Claude Code plugin structure:

```
dev-forge-plugin/
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
3. Test locally with `claude-code plugin install .`
4. Submit a pull request

## License

MIT License - see LICENSE file for details
