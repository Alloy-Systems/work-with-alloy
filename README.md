# Work with Alloy

Portable agent plugin source for working with Alloy MCP, Storage,
documentation, and Alloy operating context across Codex and Claude Code.

The shared source of truth is the `work-with-alloy` skill. Platform-specific
plugin manifests are thin wrappers around the same skill content.

## Layout

- `skills/work-with-alloy/` - shared skill source.
- `skills/work-with-alloy/references/` - supporting playbooks loaded by the skill.
- `skills/work-with-alloy/agents/openai.yaml` - Codex/OpenAI skill UI metadata.
- `.codex-plugin/plugin.json` - Codex plugin manifest.
- `.claude-plugin/plugin.json` - Claude Code plugin manifest, planned.
- `.mcp.json` - Alloy hosted MCP configuration using `ALLOY_TOKEN`.

## Shared Skill

`skills/work-with-alloy/SKILL.md` and its `references/` folder are intended to
stay host-neutral. Update this shared skill instead of maintaining separate
Claude and Codex instruction trees.

## Codex

The Codex plugin manifest references `./skills/` and `./.mcp.json`.

Set `ALLOY_TOKEN` in the host environment before using the bundled MCP config.

## Claude Code

Claude Code support should use the same `skills/work-with-alloy/` source and a
Claude-specific `.claude-plugin/plugin.json` wrapper. That manifest still needs
to be added and validated against Claude Code before publishing.
