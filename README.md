# jwhite-claude-plugin

Personal Claude Code plugin with Socratic issue creation and HumanLayer thoughts integration.

## Components

### Commands

- **`/create-issue`** - Create well-defined issues through Socratic questioning. Challenges assumptions, exposes gaps in thinking, and produces thorough issue documentation.

### Skills

- **`humanlayer-thoughts-setup`** - Guide for setting up HumanLayer thoughts using npm/Node.js for collaborative knowledge sharing.

## Installation

Install as a Claude Code plugin:

```bash
claude plugin install /path/to/jwhite-claude-plugin
```

Or add to your Claude Code configuration.

## Requirements

- Claude Code CLI
- Node.js v16+ (for HumanLayer)
- HumanLayer API key (get one at https://humanlayer.dev)

## Usage

### Create an Issue

```
/create-issue The API is returning inconsistent errors
```

The command will guide you through a Socratic questioning process to clarify:
- What exactly the problem is
- What assumptions you're making
- What the scope should be
- How to define success criteria

Issues are saved to `thoughts/shared/issues/`.

### Set Up HumanLayer Thoughts

Invoke the `humanlayer-thoughts-setup` skill to get guided installation instructions for HumanLayer using npm.

## Directory Structure

```
jwhite-claude-plugin/
  .claude-plugin/
    plugin.json         # Plugin configuration
  commands/
    create-issue.md     # Socratic issue creation command
  skills/
    humanlayer-thoughts-setup/
      SKILL.md          # HumanLayer setup guide
  README.md             # This file
  CLAUDE.md             # Claude-specific documentation
```

## License

MIT
