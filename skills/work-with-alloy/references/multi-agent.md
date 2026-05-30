# Multi-Agent Operations

Two ways to hand work to another Alloy Teammate: inside a workflow, or ad-hoc mid-conversation. Use this when a task is better handled by a Teammate with the right role, tools, or knowledge than by doing it yourself.

## Discovering Teammates

Use the employee/list Teammates tool discovered via Alloy MCP to get all Alloy AI Teammates and their UUIDs. The UUID is required for workflow delegation.

## Delegating from a workflow

Inside a workflow, the `run_employee` ("Assign AI") step delegates a chunk of work to a specific Teammate, suspends until it finishes, then resumes with its output. Use this for orchestrated, multi-step processes. For the step's parameters and how it fits the rest of the flow, see `references/workflows.md`.

## Calling a Teammate's skill ad-hoc (via MCP)

Alloy Teammates with skills may expose those skills directly as MCP tools. They appear in the deferred tool list alongside other `mcp__alloy__*` tools. Call them like any other MCP tool — no workflow needed.

Use this when you need a one-off result from a Teammate's capability mid-conversation, without building a full workflow.

## Continuing from a Teammate's output

After a `run_employee` step or an ad-hoc skill call, the Teammate's output and any artifacts it wrote to Storage are immediately available. Read relevant Storage paths to pick up where the Teammate left off before continuing your own work.
