# jwhite-claude-plugin

## Plugin Overview

This plugin provides tools for structured issue creation and HumanLayer thoughts integration.

## Commands

### /create-issue

Creates issues through Socratic questioning to expose gaps in thinking. Never writes an issue after just one exchange - pushes for 2-3 rounds of clarification.

Key behaviors:
- Challenges vague statements
- Exposes hidden assumptions
- Probes for edge cases
- Questions scope
- Verifies understanding through playback

Issues are saved to `thoughts/shared/issues/YYYY-MM-DD-kebab-case-title.md`.

After creating an issue, sync with: `humanlayer thoughts sync`

## Skills

### humanlayer-thoughts-setup

Guides users through HumanLayer setup using npm (not Python):

```bash
npm install -g hlyr
export HUMANLAYER_API_KEY=your_key
humanlayer thoughts sync
```

## Integration Notes

- The `/create-issue` command expects `thoughts/shared/issues/` directory to exist
- Run `humanlayer thoughts sync` after creating issues to share with team
- HumanLayer can run as MCP server: `humanlayer mcp serve`
