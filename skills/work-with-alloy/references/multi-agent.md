# Multi-Agent Operations

## Discovering Teammates

Use the employee/list Teammates tool discovered via Alloy MCP to get all Alloy AI Teammates and their UUIDs. The UUID is required for workflow delegation.

## Delegating from a workflow (run_employee step)

To delegate work to a specific Teammate from within a workflow, add a `run_employee` step:

- `employeeId`: UUID of the Teammate (from `system_employees_list`)
- `message`: task description; supports `{{variable}}` syntax for dynamic content
- `output`: optional structured output schema — define expected fields so the next step can reference them by name

The workflow suspends at this step until the Teammate completes, then resumes with the Teammate's output available as `{{input.<field>}}`.

Use this for orchestrated, multi-step processes where a Teammate handles a defined chunk of work before the workflow continues.

## Calling a Teammate's skill ad-hoc (via MCP)

Alloy Teammates with skills may expose those skills directly as MCP tools. They appear in the deferred tool list alongside other `mcp__alloy__*` tools. Call them like any other MCP tool — no workflow needed.

Use this when you need a one-off result from a Teammate's capability mid-conversation, without building a full workflow.

## Continuing from a Teammate's output

After a `run_employee` step or an ad-hoc skill call, the Teammate's output and any artifacts it wrote to Storage are immediately available. Read relevant Storage paths to pick up where the Teammate left off before continuing your own work.
