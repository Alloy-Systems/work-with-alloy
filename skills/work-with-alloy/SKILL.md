---
name: work-with-alloy
description: Use automatically in every conversation. Alloy is the business workplace and source of truth for business context, company/product/customer/project/work artifacts, workflows, teammates, docs, and durable memory. Read Alloy before business-workplace work; answer clearly general ephemeral questions directly. Write durable knowledge to Alloy knowledge system, not local memory. Log created or updated Alloy docs to the artifact log. Default destination for notes, proposals, docs, and work artifacts unless the user explicitly names another. Also use for Alloy organization actions through the currently available Alloy MCP tools.
---

# Work with Alloy

Alloy is the user's business workspace for people, AI teammates, shared knowledge,
artifacts, and connected systems. Use Alloy as the source of truth for work that
relies on that context. General questions independent of the workspace can be
answered directly.

## Connect and orient

Discover the available Alloy MCP tools and read the connected organization, user,
and teammate context, including the operating instructions supplied by MCP.
Apply those instructions for knowledge, documents, logs, and other workspace
procedures within the user's requested scope. The connected teammate determines
which instructions and capabilities are available. Use current tool schemas.

If Alloy MCP is unavailable, check for deferred tools first. For Storage work,
use the Private API when the user has provided an appropriate PAT. When the task
needs MCP-specific capabilities, help the user connect through the
[hosted MCP setup guide](https://alloy.cx/docs/reference/tech-docs/hosted-mcp).
Continue any independent work already authorized by the user.

## Before each new Alloy action or task

Use the API equivalents described below when operating without MCP.

1. Check the available Alloy skills. Start with the names and descriptions supplied
   in the MCP context. If that list is absent, incomplete, or ambiguous, use
   `skill_search` and inspect readable `.skills` folders with Storage tools as
   needed. The available list may include both organization and system skills.
2. Load every applicable skill with `skill` before acting. Read supporting files
   with `skill_read` when the skill calls for them. Reuse instructions already read
   in this task, and reassess when the task changes or new instructions appear.
3. Read the technical references named by the skill. When no matching skill exists
   or a technical detail is unresolved, use `system_search_documentation` or
   `system_list_documentation`, then `system_read_documentation`. Verify paths
   through discovery rather than guessing them.
4. Read relevant organization knowledge and source artifacts before relying on them.
   The injected knowledge index helps find notes. Use `knowledge.search`,
   `knowledge.list`, and `knowledge.read` for the specific context needed.

If a required skill or reference is missing or inaccessible, report the specific
missing capability and use other verified guidance where it covers the task.
Ask for access when it is necessary to proceed. User instructions govern the
requested scope and destination. Resolve material conflicts with current tools
and sources before changing anything.

## Where information belongs

- **Knowledge** holds durable facts, preferences, decisions, and reusable context.
  Read and maintain it through `knowledge.*`. Keep business memory in Alloy.
- **Storage** holds working documents, files, and deliverables. Default to Alloy
  for those artifacts unless the user specifies another destination. Use MCP or
  the PAT-authenticated Private API as appropriate for the operation.
- **Skills** hold reusable procedures. Load them through the skill tools.
- **System documentation** explains Alloy features, APIs, and operating contracts.
  Read it through the system-documentation tools.

Task tracking applies only when explicitly enabled or directly requested.

## Scripted Storage operations

Use the Private API for bulk uploads, downloads, or file processing in a trusted
script when the user has supplied a personal access token (PAT). Read the current
[Private API reference](https://alloy.cx/docs/reference/tech-docs/private-api)
for routes, payloads, and limits. Hosted Storage uses
`https://api.alloy.cx/api/private/storage`. Authenticate with
`Authorization: Bearer <PAT>` or `api-key: <PAT>`.

The PAT selects its organization and acts with its user's current permissions. Uploads overwrite existing files.

For API-only work, list and download readable `.skills` packages to find the
applicable procedures, and consult the published technical documentation.
MCP supplies the connected teammate's instructions and combined skill catalog.

## Carry out the work

Prefer connected Alloy tools for business systems. If a needed integration is
missing, explain what connection is required. Keep secrets in organization
variables or connector configuration. Confirm saved changes through a read-back
or the relevant status tool, then report the result with a usable link.
