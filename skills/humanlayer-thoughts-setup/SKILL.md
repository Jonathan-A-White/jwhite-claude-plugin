# HumanLayer Thoughts Setup

This skill guides Claude through setting up HumanLayer thoughts for collaborative knowledge sharing and issue tracking.

## Overview

HumanLayer thoughts is a system for syncing shared context (issues, plans, notes) across team members. The `thoughts/` directory in a repo is symlinked to a central `~/thoughts` repo, enabling cross-project knowledge sharing.

## Prerequisites

Before starting, ensure you have:
- Node.js installed (v16 or later recommended)
- npm available in your PATH
- A HumanLayer API key (get one at https://humanlayer.dev)
- Git configured with user.name and user.email

## Installation

### Step 1: Install HumanLayer CLI Globally

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

## Setting Up Thoughts for a Repository

### Step 1: Clone the Thoughts Repo

First, check if `~/thoughts` already exists:

```bash
ls -la ~/thoughts
```

If not, clone the org's thoughts repo. Check remotes to find the org:

```bash
# Check for upstream remote first
git remote get-url upstream 2>/dev/null

# If no upstream, check origin
git remote get-url origin
```

Extract the org name and clone the thoughts repo:

```bash
# Example: if origin is git@github.com:myorg/myrepo.git
# Clone git@github.com:myorg/thoughts.git to ~/thoughts
git clone git@github.com:<org>/thoughts.git ~/thoughts
```

If neither remote has a thoughts repo, ask the user for the thoughts repo URL.

### Step 2: Initialize Thoughts Symlinks

The `humanlayer thoughts init` command is interactive. Run it non-interactively:

```bash
# Get the repo name from the current directory
REPO_NAME=$(basename $(git rev-parse --show-toplevel))

# Run init non-interactively (option 2 = create new repo config)
(sleep 0.5; echo "2"; sleep 0.5; echo "$REPO_NAME") | humanlayer thoughts init 2>&1 | cat
```

This creates symlinks in the current repo:

```
<repo>/thoughts/
  ├── global/   -> ~/thoughts/global
  └── shared/   -> ~/thoughts/repos/<repo>/shared
```

**Note:** Personal thoughts symlink (e.g., `<username>/`) is not created by default. Only add if user explicitly requests it. Get username via `whoami` or `$USER`.

### Step 3: Verify Setup

```bash
ls -la thoughts/
# Should show symlinks pointing to ~/thoughts/...
```

## Symlink Architecture

The thoughts system uses symlinks to separate concerns:

| Directory | Points To | Purpose |
|-----------|-----------|---------|
| `thoughts/global/` | `~/thoughts/global` | Cross-project knowledge, shared across all repos |
| `thoughts/shared/` | `~/thoughts/repos/<repo>/shared` | Project-specific shared context |
| `thoughts/<username>/` | `~/thoughts/repos/<repo>/<username>` | Personal notes (optional, on request) |

The actual files live in `~/thoughts/` which is its own git repo, synced separately from the main codebase.

## Workflow Rules

### Critical: Never Commit Thoughts Directly

The `thoughts/` directory contains symlinks to `~/thoughts/repos/<repo>/`. **Do not** run `git add thoughts/` in the main repo.

Git hooks are installed automatically by `humanlayer thoughts init`:
- **Pre-commit hook**: Prevents accidentally committing thoughts/ files
- **Post-commit hook**: Runs `humanlayer thoughts sync` automatically

### Autosync Behavior

When you commit to the main repo, the post-commit hook automatically syncs thoughts. No manual sync needed for normal workflow.

### Manual Sync

If you need to sync thoughts without committing:

```bash
humanlayer thoughts sync
```

## Available Agents

HumanLayer provides specialized agents for codebase and thoughts analysis. Use these via the Task tool:

| Agent | Description | Tools |
|-------|-------------|-------|
| `codebase-analyzer` | Analyzes code implementation details, documents how code works with file:line references | Read, Grep, Glob, LS |
| `codebase-locator` | Locates files and components relevant to a feature. "Super Grep/Glob/LS" | Grep, Glob, LS |
| `codebase-pattern-finder` | Finds similar implementations and usage examples | Grep, Glob, Read, LS |
| `thoughts-analyzer` | Deep research document analysis, extracts actionable insights | Read, Grep, Glob, LS |
| `thoughts-locator` | Discovers relevant documents in thoughts/ directory | Grep, Glob, LS |
| `web-search-researcher` | Web research for modern/external information | WebSearch, WebFetch, TodoWrite, Read, Grep, Glob, LS |

### Using Agents

```
Use the Task tool with subagent_type="codebase-locator" to find files related to authentication
```

## Directory Structure

After setup, the thoughts directory contains:

```
thoughts/
  ├── CLAUDE.md           # Local instructions (not symlinked)
  ├── global/             # -> ~/thoughts/global
  │     └── ...           # Cross-project knowledge
  └── shared/             # -> ~/thoughts/repos/<repo>/shared
        ├── issues/       # Issue definitions (created via /create-issue)
        ├── plans/        # Implementation plans
        └── notes/        # General notes and context
```

## Configuration Files

| File | Purpose |
|------|---------|
| `~/.config/humanlayer/humanlayer.json` | Global HumanLayer config (written by init) |
| `.hlyr.json` | Project-specific settings (optional) |

## Optional: Notification Channels

### Slack Integration

```bash
export HUMANLAYER_SLACK_CHANNEL=C08XXXXXXXX
```

### Email Integration

```bash
export HUMANLAYER_EMAIL_ADDRESS=your@email.com
```

## MCP Server Mode

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
| `npm install -g hlyr` | Install HumanLayer CLI |
| `humanlayer thoughts init` | Initialize thoughts for current repo |
| `humanlayer thoughts sync` | Sync thoughts directory |
| `humanlayer contact_human -m "msg"` | Contact a human |
| `humanlayer mcp serve` | Start MCP server |

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
# Fix npm permissions (recommended)
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
npm install -g hlyr
```
