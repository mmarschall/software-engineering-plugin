# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Claude Code plugin that provides development tools and workflows. It follows the standard Claude Code plugin structure with commands, agents, skills, hooks, and scripts.

## Plugin Architecture

### Core Plugin Structure

- **Plugin Manifest**: `.claude-plugin/plugin.json` - Defines plugin metadata, version, and hook configuration path
- **Hooks Configuration**: `hooks/hooks.json` - Event handlers that execute commands in response to Claude Code events

### Component Directories

1. **commands/** - Slash commands that users invoke with `/command-name`
   - Each command is a markdown file with frontmatter (name, description) followed by the command prompt
   - When invoked, the markdown content is expanded and sent to Claude

2. **agents/** - Specialized subagent definitions
   - Markdown files defining specialized capabilities for specific task types
   - Use frontmatter for metadata (name, description)

3. **skills/** - Reusable capabilities
   - Each skill in its own subdirectory with a `SKILL.md` file
   - Skills provide capabilities Claude can use during conversations
   - Use frontmatter for metadata and structured sections (When to Use, How It Works, Examples)

4. **hooks/** - Event handlers
   - Currently configured hooks:
     - **Notification**: Triggers when Claude sends a notification (uses macOS `say` command with Ava voice)
     - **Stop**: Triggers when Claude stops processing (uses macOS `say` command with Ava voice)
   - Hook definitions include matcher patterns and command specifications

5. **scripts/** - Executable utilities
   - Referenced by hooks and other plugin components
   - Can be shell scripts, Python, or any executable

## Plugin Installation and Testing

Install the plugin locally for development:
```bash
# Add current directory as a local marketplace
/plugin marketplace add .

# Install the plugin from the local marketplace
/plugin install software-engineering-plugin@.
```

Or install from GitHub:
```bash
# Add this repository as a marketplace
/plugin marketplace add mmarschall/software-engineering-plugin

# Install the plugin
/plugin install software-engineering-plugin@software-engineering-marketplace
```

## Creating New Components

### Adding a Slash Command
Create a markdown file in `commands/` with frontmatter:
```markdown
---
name: command-name
description: What this command does
---

Command implementation prompt here.
```

### Adding an Agent
Create a markdown file in `agents/` with frontmatter:
```markdown
---
name: agent-name
description: Brief description of agent capabilities
---

Agent instructions and capabilities.
```

### Adding a Skill
Create a subdirectory in `skills/` with a `SKILL.md` file:
```markdown
---
name: skill-name
description: Brief description
---

# Skill Name

## When to Use
[Describe scenario]

## How It Works
[Implementation details]

## Example
[Usage examples]
```

### Adding a Hook
Edit `hooks/hooks.json` to add new event handlers. Available events include Notification, Stop, and others from Claude Code's hook system.

## Platform Notes

This plugin currently uses macOS-specific voice commands (`say -v 'Ava'`) in hooks. When extending for cross-platform support, consider platform detection or alternative notification mechanisms.
