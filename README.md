# Work with Alloy

Codex plugin and portable agent skill for working with Alloy MCP, Storage,
documentation, and Alloy operating context.

## Layout

- `.codex-plugin/plugin.json` - Codex plugin manifest.
- `.mcp.json` - Alloy hosted MCP configuration using `ALLOY_TOKEN`.
- `skills/work-with-alloy/` - shared skill source.
- `skills/work-with-alloy/references/` - supporting playbooks loaded by the skill.

## Codex

The Codex plugin manifest references `./skills/` and `./.mcp.json`.

Set `ALLOY_TOKEN` in the host environment before using the bundled MCP config.
