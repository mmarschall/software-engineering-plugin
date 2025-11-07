# Hooks

This directory contains hook configuration files for event handling.

Hooks allow you to execute scripts or commands in response to specific events in Claude Code.

## Example

Create a `hooks.json` file to define your hooks:

```json
{
  "pre-commit": "./scripts/pre-commit.sh",
  "post-session": "./scripts/cleanup.sh"
}
```
