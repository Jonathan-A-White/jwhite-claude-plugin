# HumanLayer Thoughts Setup

This skill guides users through setting up HumanLayer thoughts for collaborative knowledge sharing and issue tracking.

## Overview

HumanLayer thoughts is a system for syncing shared context (issues, plans, notes) across team members. This skill helps you install and configure it using npm (Node.js).

## Prerequisites

Before starting, ensure you have:
- Node.js installed (v16 or later recommended)
- npm available in your PATH
- A HumanLayer API key (get one at https://humanlayer.dev)

## Installation Steps

### Step 1: Install HumanLayer CLI Globally

Install the HumanLayer CLI using npm:

```bash
npm install -g hlyr
```

Verify the installation:

```bash
humanlayer --version
```

### Step 2: Configure API Key

Set your HumanLayer API key as an environment variable. Add this to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.):

```bash
export HUMANLAYER_API_KEY=your_api_key_here
```

Then reload your shell or run:

```bash
source ~/.bashrc  # or ~/.zshrc
```

### Step 3: Initialize Thoughts Directory

Create the thoughts directory structure in your project:

```bash
mkdir -p thoughts/shared/issues
mkdir -p thoughts/shared/plans
mkdir -p thoughts/shared/notes
```

### Step 4: Configure Thoughts Sync

You can sync thoughts using:

```bash
humanlayer thoughts sync
```

For automatic syncing, consider adding a git hook or running sync as part of your workflow.

## Optional: Configure Notification Channels

### Slack Integration

To receive notifications via Slack:

```bash
export HUMANLAYER_SLACK_CHANNEL=C08XXXXXXXX
```

### Email Integration

To receive notifications via email:

```bash
export HUMANLAYER_EMAIL_ADDRESS=your@email.com
```

### Configuration File

For project-specific settings, create `.hlyr.json` in your project root:

```json
{
  "slack_channel": "C08XXXXXXXX",
  "email": "team@example.com",
  "thoughts_dir": "thoughts/shared"
}
```

## Directory Structure

After setup, your thoughts directory should look like:

```
thoughts/
  shared/
    issues/        # Issue definitions (created via /create-issue)
    plans/         # Implementation plans
    notes/         # General notes and context
```

## Usage with Claude Code

Once set up, you can use the `/create-issue` command to create issues through Socratic questioning. Issues are automatically saved to `thoughts/shared/issues/` and can be synced with your team.

### Common Commands

```bash
# Sync thoughts with team
humanlayer thoughts sync

# Contact a human for approval/input
humanlayer contact_human -m "Need approval for deployment"

# Run without installation (one-off usage)
npx humanlayer contact_human -m "Quick question"
```

## Troubleshooting

### Command not found after installation

If `humanlayer` is not found after `npm install -g hlyr`:

1. Check your npm global bin path: `npm bin -g`
2. Ensure this path is in your `$PATH`
3. Try running with npx: `npx humanlayer --version`

### API Key Issues

If you get authentication errors:

1. Verify your API key is set: `echo $HUMANLAYER_API_KEY`
2. Ensure no extra whitespace in the key
3. Generate a new key at https://humanlayer.dev if needed

### Permission Errors

If you get permission errors during global install:

```bash
# Option 1: Use sudo (not recommended)
sudo npm install -g hlyr

# Option 2: Fix npm permissions (recommended)
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
npm install -g hlyr
```

## Integration with Other Tools

### MCP Server Mode

HumanLayer can run as an MCP server for deeper Claude Code integration:

```bash
humanlayer mcp serve
```

For approval workflows:

```bash
humanlayer mcp claude_approvals
```

## Quick Reference

| Command | Description |
|---------|-------------|
| `npm install -g hlyr` | Install HumanLayer CLI globally |
| `humanlayer thoughts sync` | Sync thoughts directory |
| `humanlayer contact_human -m "msg"` | Contact a human |
| `humanlayer mcp serve` | Start MCP server |
| `npx humanlayer <cmd>` | Run without global install |
