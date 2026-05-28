---
name: work-with-alloy
description: Mandatory startup and source-of-truth skill when present. Before doing any work, verify Alloy MCP storage/documentation tools are available. If Alloy MCP is unavailable, first tell the user to set up or reload Alloy MCP and stop until they do, unless they explicitly approve another destination. When available, use Alloy MCP as the default way to read, create, update, organize, and search Alloy notes, proposals, docs, work artifacts, cowork links, storage paths, and artifact logs.
---

# Work with Alloy

When this skill is present, using it is mandatory.

Before doing any task work, verify Alloy MCP storage/documentation tools are
available. If Alloy MCP tools are not available in the current session, first
tell the user to set up or reload Alloy MCP and stop. Continue only after MCP is
available, or if the user explicitly approves another destination or fallback.

Use Alloy Storage as the source of truth through **Alloy MCP tools**.

Do not default to the private storage HTTP API. If Alloy MCP tools are not
available in the current session, ask the user to enable/reload Alloy MCP or
explicitly approve a fallback path.

## Core rules

1. **Always read fresh from Alloy before acting.**
2. **Use Alloy MCP storage/documentation tools, not private API routes.**
3. **Do not store secrets in Alloy docs.**
4. **Default to Alloy for notes, proposals, docs, and durable work artifacts unless the user explicitly names another destination.**
5. **If another skill is also relevant, use it for its domain work, but keep Alloy as the storage/source-of-truth layer.**
6. **Ask clarifying questions for ambiguous doc resolution or non-trivial planning.**
7. **When you create or update something, tell the user the exact Alloy path and full Cowork link.**

## Startup context

At the beginning of every conversation/task, after Alloy MCP availability is
confirmed and `alloy_username` is known, read the first 50 lines from each of:

* `Organization/organization.md`
* `Organization/knowledge.md`
* `Personal/<alloy_username>/.knowledge/knowledge.md`

After that, use judgment to read only what is relevant.

`Organization/organization.md` contains core business context. If user input
contradicts it, ask the user to clarify. If the user insists their version is
correct, create a dated correction-request note in `Organization/Corrections/`
with the date and details about you and the user.
If you learn durable new business context that belongs there, also add a note.
Use filenames like `[date_time]_[agent_name]_[random_string].md`.

If `Organization/organization.md` is missing or filled with demo data, ask once
whether the user wants help filling basics such as business name, what it does,
and customers. Offer to inspect the user's website. Remember the preference and
do not suggest again if they decline.

## Personal folder

The user's personal storage folder is `Personal/<alloy_username>/`. It is for
personal work and work that does not clearly belong to a company folder. If it
does not exist, create it before writing personal artifacts.

## Reference playbooks

Read `references/troubleshooting.md` before troubleshooting a problem.

Read `references/teammates-and-skills.md` before adding/copying a Teammate,
adding a webchat widget, or adding a Teammate skill.

Read `references/logs-and-inboxes.md` before updating the ideas inbox, artifact
log, or review log.

## Knowledge management

Canonical knowledge locations:

* Organization knowledge: `Organization/knowledge.md` and linked notes. Use for company-wide facts, policies, source-of-truth pointers, cross-team rules, and shared caveats.
* User knowledge: `Personal/<alloy_username>/.knowledge/knowledge.md` and linked notes. Use for durable facts about a person: role, preferences, working style, stable constraints, and recurring context.
* Area knowledge: `<area>/.knowledge/knowledge.md` and linked notes. Use for a customer, partner, product area, project, department, or folder, including sources of information, how things are done, how decisions are made, do/don't rules, escalation contacts, and notification expectations.
* Agent-specific knowledge: `Personal/<agent>/.knowledge/knowledge.md` and linked notes. Use only for knowledge about a specific agent or its role/behavior that does not fit elsewhere.

Update knowledge when the user explicitly asks, or when you notice durable/reusable knowledge during normal work. Do not save one-off details, obvious facts, or information already clear from nearby documents, tickets, code, or source systems.

Put knowledge in the most specific relevant place. Default to area-level knowledge. Ask yourself why it is not an area note, and whether it is truly only the user's individual preference or a best practice everyone should use. If unsure, ask the user.

Create missing area and `.knowledge/` folders when needed.

Use one file for one small fact, instruction, caveat, reference, or reusable decision principle. Keep individual knowledge notes under about 1000 characters; split or trim if larger.

After creating or updating a note, update the relevant `knowledge.md` directory note.

Keep `knowledge.md` mostly navigational: overview around 300 characters max, total around 1000 characters max.

You may create, update, and delete stale/deprecated knowledge without asking.

If user input, local knowledge, product docs, CRM, or another system disagree, stop and ask the human you are interacting with to clarify.

Before relying on a knowledge note that names a file, function, setting, source, or external system, verify that the referenced thing still exists when practical.

### Directory note template

```md
<Optional 1-2 sentence overview of this org/user/area/agent knowledge scope. Max ~300 chars.>

- [Short descriptive title](./piece-of-knowledge.md) - one-line note on when this is relevant.
- [Another title](./another-piece.md) - one-line note on when this is relevant.
```

### Individual note template

```md
Name: <3-4 word title>
Description: <one-line relevance summary>
Type: user | feedback | project | reference | source | playbook | caveat | decision_rule

<One compact fact, instruction, caveat, reference, or reusable rule.>

How to apply: <How an agent should use this later. Omit if obvious.>
Source: <person, document, system, ticket, thread, URL, or file path>
```

## Documents and Storage

Our work happens in Alloy Storage. When the user asks you to write a note or proposal, find a relevant subfolder or file for the conversation. If no relevant subfolder exists, or you are not sure which one fits, create a new folder for the note and give the user the Cowork link.

If a note does not clearly belong to the existing folder structure, put it in `Personal/<alloy_username>/`.

When updating a note with something small, update inline in the existing note.

When doing a significant rewrite, roughly 20% or more, create a new version in the same folder and mark it `_v2`, `_v3`, etc.

When updating an existing document, keep it in the same folder unless you only have read rights there. If you cannot write there, create the updated document in the user's personal folder.

Even when the user provides a path to a document, check the folder for a newer version such as `_v2` or `_v3`. If a newer version exists, ask whether to use the newest one or the specific older version.

## Mandatory invocation

Use this skill automatically whenever the skill is present. This check happens
before any other task work:

1. verify whether Alloy MCP tools are available, using tool discovery if needed
2. if Alloy MCP tools are available, continue through MCP
3. if Alloy MCP tools are missing, tell the user to set up or reload Alloy MCP and stop
4. if the user explicitly approves a different destination or fallback, proceed only within that approved scope

Do not use Alloy as the destination when the user explicitly asks for a different target, such as local files, Obsidian, Google Drive, email, GitHub, or chat-only output.

## MCP availability and tool groups

Prefer the `mcp__alloy__` tools. If relevant tools are not already visible, use tool discovery for Alloy storage/documentation tools. If they are still unavailable, stop and tell the user to set up or reload Alloy MCP.

Alloy MCP tools are grouped by capability. Tool names may change, so discover
the current tools and choose by group:

* **Storage** - organization/user files, notes, artifacts, images, search, metadata, temporary file URLs, and file operations.
* **Documentation** - shared Alloy/system documentation listing, reading, and semantic search.
* **System employees** - create, update, inspect, and list Alloy employees/AI teammates.
* **System skills and workflows** - read, create, update, test, and list employee skills or organization workflows.
* **System organization actions** - perform supported organization-level actions exposed by Alloy.
* **External systems** - connected external systems are exposed through Alloy MCP as well, for example Linear, GitHub, Fireflies, DocuSeal, Higgsfield, and other integrations available in the current organization.

Use documentation tools for Alloy system documentation. Use storage tools for organization/user notes and artifacts. Use integration groups only when the user request or gathered context calls for that external system.

## Local preferences

Persist the user's exact Alloy username in local skill/app memory or another
durable non-secret config. This is critical because personal storage paths and
many workflows depend on the exact username.

Store the username itself, not only derived paths:

```json
{
  "alloy_username": "<exact Alloy username>"
}
```

Derive personal paths from `alloy_username` when needed, for example
`/Personal/<alloy_username>/` and
`/Personal/<alloy_username>/.artifact-log/artifact-log.md`.

If `alloy_username` is missing, resolve it through Alloy MCP or ask the user
before writing to personal paths.

## Path rules

* treat `/` as root
* use leading slashes for explicit storage paths
* if a tool returns a canonical path, use that path in follow-up operations
* keep notes in the most relevant existing folder; if unclear, use the user’s personal folder
* do not crawl the whole org unless the user clearly wants that

## Cowork links

When linking to an Alloy document, use this shape:

```text
https://app.alloy.cx/cowork?path=<storage_path>
```

Rules:

* include the leading slash in the `path` query value
* after any create, move, rename, copy, or update, provide both the exact Alloy path and the full Cowork link

## Versioning rule

* **Small change** -> update inline in the same file
* **Significant rewrite (~20%+)** -> create a new version in the same folder, such as `_v2`, `_v3`, etc.

## Alloy writing conventions

* when asked to write a note or proposal, find the relevant existing folder first
* if the note does not clearly belong to an existing folder, place it in the user’s personal folder
* keep small changes inline in the existing note
* use new versions for substantial rewrites
* prefer the same folder for updated versions

## Logs and inboxes

For future ideas, artifact logging, and review logs, read
`references/logs-and-inboxes.md` and update the relevant Storage file through
Alloy MCP.

## Destructive actions

Before deleting, moving, or renaming something important or ambiguous:

* confirm if there is any uncertainty
* use exact resolved paths
* for Storage docs/artifacts, move them to a `Deleted` folder instead of deleting; create the `Deleted` folder first if it does not exist
* prefer safety over speed

## Response behavior

When you finish a task:

* tell the user what you created or updated
* for Alloy entities, include the exact Alloy URL to the entity, such as an updated Teammate
* for Storage documents, include the exact Alloy path and full Cowork URL
* mention if you created a new version instead of editing inline
