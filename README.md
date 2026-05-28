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
- `.claude-plugin/plugin.json` - Claude Code plugin manifest.
- `.mcp.json` - Alloy hosted MCP configuration using `ALLOY_TOKEN`.

## Shared Skill

`skills/work-with-alloy/SKILL.md` and its `references/` folder are intended to
stay host-neutral. Update this shared skill instead of maintaining separate
Claude and Codex instruction trees.

## Prerequisites

These apply to anyone installing the plugin into Claude Code or Codex.

### 1. `ALLOY_TOKEN` environment variable

The bundled `.mcp.json` reads the Alloy MCP bearer from `ALLOY_TOKEN`. Export
it in the host shell before starting Claude Code or Codex:

```
export ALLOY_TOKEN="<your-alloy-token>"
```

Get a token at https://alloy.cx/docs/reference/tech-docs/hosted-mcp.

### 2. Host restart after install

The plugin's MCP server is loaded by the host at startup. After installing or
updating the plugin, restart the host so it can load the bundled MCP config.

## Codex

The Codex plugin manifest references `./skills/` and `./.mcp.json`.

Set `ALLOY_TOKEN` in the host environment before using the bundled MCP config.

The Codex install and marketplace flow still needs end-to-end validation.

## Claude Code

The Claude plugin manifest at `.claude-plugin/plugin.json` references the same
`./skills/` and `./.mcp.json` as the Codex side. Install flow once the Alloy
marketplace is published:

```
/plugin marketplace add alloy-cx/marketplace
/plugin install work-with-alloy
```

Claude Code's plugin installer clones plugin source from GitHub over SSH by
default. Before `/plugin install`, make sure one of these is true:

- A working GitHub SSH key on this machine (test: `ssh -T git@github.com`).
- A global git URL rewrite that pushes plugin clones through HTTPS:

  ```
  git config --global url."https://github.com/".insteadOf "git@github.com:"
  ```

Set `ALLOY_TOKEN` in the host environment before using the bundled MCP config.

After install, restart Claude Code and confirm registration:

```
claude mcp list   # expect plugin:work-with-alloy:alloy as connected
```

## Versioning policy

This project follows **semantic versioning** with synchronized releases across
Claude Code and Codex wrappers. A single version applies to the whole release:
the shared skill content in `skills/work-with-alloy/`, both `plugin.json`
manifests, the `.mcp.json` config, and this README. Both
`.codex-plugin/plugin.json` and `.claude-plugin/plugin.json` always carry the
same `version` field.

### Semver semantics for this plugin

| Bump | What it means | Examples |
|------|---------------|----------|
| **Patch** (`0.1.0` → `0.1.1`) | Cosmetic only. Typo, wording tweak, broken link, clarification. No change in how the agent behaves. | Fix a typo in `SKILL.md`. Update a stale URL in `references/`. |
| **Minor** (`0.1.0` → `0.2.0`) | Backward-compatible addition. New rule, new Quick Reference row, new reference playbook, new optional manifest field. Prior usage still works. | Add a new rule to Core rules. Add a new entry to a Quick Reference table. Add a new playbook under `references/`. |
| **Major** (`0.x` → `1.0`, `1.x` → `2.x`) | Breaking change. A canonical path, file name, required field, or required rule changes in a way that prior consumers may break. | Rename `Personal/<username>/.knowledge/knowledge.md` to a different path. Remove a Core rule. Restructure `references/` so existing links break. |

Pre-1.0 (`0.x`) signals "still stabilizing - minor bumps may include breaking
changes". The `1.0.0` line will be cut when the canonical paths and Core rules
are confirmed stable.

### Release flow

1. Work happens on feature branches, merged into `main` via PR.
2. When `main` is in a state worth releasing, bump both
   `.codex-plugin/plugin.json` and `.claude-plugin/plugin.json` `version`
   fields in the same commit.
3. Cut a git tag on the merge commit: `git tag vX.Y.Z && git push origin vX.Y.Z`.
4. Bump the corresponding `ref` field in the `alloy-cx/marketplace` repo's
   `marketplace.json` entry for `work-with-alloy`. Users receive the new
   version through `/plugin update` only after this marketplace bump.
5. Untagged commits on `main` are visible to anyone reading the repo but are
   not delivered through `/plugin install` or `/plugin update` until the
   marketplace points at a new tag.

### Cadence

Tags are cut per logical release, not on every commit. A typical trigger is a
new Core rule landing, a new reference playbook being added, or a breaking
restructure being completed and ready to ship. Cosmetic-only changes can
accumulate in `main` and be released as a patch bump when convenient.
