# Building Workflows

Alloy workflows are directed graphs of steps that combine deterministic code, LLM calls, and AI Teammate ("agent") calls. They run on triggers (public/private API, schedule) or manually, with or without an active session.

When building a workflow for a user, act as a **design advisor first** — help them make good implementation choices before writing anything.

## When to build a workflow

- Repeated automation that should run the same way every time
- Work triggered by an external event (webhook, schedule, API call)
- Multi-step pipelines moving data between systems
- Work that must run without an active chat session

## The building process

For non-trivial workflows, preview the design before building. If the user already gave a clear design, summarize assumptions and proceed.

1. **Ask.** Clarify before designing:
   - What is the goal and what does "done" look like?
   - What triggers it (manual, schedule, webhook/API) and what inputs arrive?
   - What are the outputs and where do they go?
   - How often are the steps likely to change?
   - How sensitive is this to cost and reliability vs. flexibility?
2. **Design.** Choose step types using the decision logic below.
3. **Preview.** Present the proposed design to the user with trade-offs — the pros and cons of the key choices (especially deterministic vs. agent steps). For non-trivial workflows, get approval before building.
4. **Build.** Only after the user approves the design.
5. **Test & troubleshoot.** Run via the workflow/skill test tool discovered via Alloy MCP, fix failures, retest. Return to the user when it works or when you need an action from them.

## Flow size

Keep each workflow's `flow` JSON around **10,000 characters or less**. If the flow requires more, split it into chained flows or child flows by responsibility, phase, or decision branch. This improves debuggability, maintainability, and partial changes.

## Choosing the step type — the central trade-off

**Default to deterministic.** Code, tool calls, and conditions are cheaper, faster, more reliable, and predictable. The more of the workflow you can cover deterministically, the better. Start here and only reach for an LLM or agent step when there is a concrete reason.

**Use an LLM step** when you need language understanding or generation but no tools and no memory — classification, extraction, drafting, summarizing — and the logic is stable. An LLM step is a single stateless prompt; it is more predictable and cheaper than an agent step.

**Use an agent step** (`run_employee`, shown as "Assign AI" in the builder) when one of these holds:
- The steps are expected to change often and the user would rather control behavior through agent instructions than by editing code.
- The task cannot be described deterministically.
- A deterministic implementation would be too expensive or complex, and the user is unsure of the exact logic.
- The step genuinely needs tools, judgment, and multi-turn reasoning together.

Voice these as recommendations to the user. For example: "We can do this step in code — it's cheaper and won't drift — but if you expect to tweak the rules often, an Assign AI step lets you change behavior in instructions instead. Which fits how you'll maintain this?"

## Step types

**Deterministic**
- `run_code` — Python or JavaScript in an isolated sandbox. Best for calculations, parsing, data shaping, calling external APIs via libraries.
- `tool` — executes one backend tool (e.g. `http.http_request`) after parameter substitution.
- `condition` — branches on comparisons; first matching condition wins, with a default branch.

**LLM**
- `llm` — single stateless prompt to a model. No tools, no memory between calls. Optional structured `output` schema.

**Agent**
- `run_employee` ("Assign AI") — assigns work to a specific AI Teammate (by UUID), suspends until it completes, resumes with its output. Supports attachments and an optional declared output schema. See `references/multi-agent.md` for delegation details.

**Flow control**
- `user_input` — suspends and waits for a user response.
- `send_message` — sends a message and continues.
- `run_workflow` — runs a child workflow and resumes with its result.
- `starting_step` — entry point; defines the trigger payload format.
- `finish` — terminates the branch.

## Data flow

Reference variables with `{{source.path}}`. Always include a path after the source — `{{input}}` alone does not resolve.

- `{{input.field}}` — output of the **direct** previous step (or the trigger payload for the first step). When a step has multiple predecessors, the referenced field must exist in *all* of them.
- `{{state.key}}` — persistent key/value storage shared across all steps. Write with a step's `saveToState` array; read anywhere downstream. Use this when data is needed by a non-adjacent step.
- `{{output.field}}` — the current step's own result; only valid inside `saveToState`.
- `{{VARIABLE_NAME}}` — organization-level variables (API keys, secrets, config). No dot prefix. Managed in org settings, never hard-coded.

## Parent/child flows

Treat `run_workflow` as an explicit contract boundary.

- Pass values the child needs through the child workflow's payload mapping. Do not assume a child can see arbitrary parent step outputs unless you mapped them into the child payload or intentionally stored them in shared `state`.
- Use `state` for values that must survive across non-adjacent steps or be reused in several places. Use child payload for one-off inputs to that child.
- After the child workflow finishes, the parent resumes with the child's result. The next parent step reads that result through `{{input.*}}`.
- If the child depends on shared `state`, define compatible state keys intentionally in the child workflow. Avoid relying on undeclared or accidental state shape.

## Storage access from workflows

When a workflow needs to read from or write to Alloy Storage and there is no direct Storage step available in the workflow builder, use the Alloy private Storage API.

- Use `run_code` or `http.http_request` to call `{api_url}/api/private/storage`.
- Authenticate with the organization private Alloy API key in the `api-key` header.
- Keep the API key in an organization variable, never hard-code it in the workflow or prompt. If the key is not already available as a variable, ask the user to add it before building the workflow.
- Use the private Storage API for file/folder operations such as listing folders, uploading files, reading metadata, downloading files, deleting files or folders, creating folders, renaming, and moving.
- For exact route shapes, look up the current Alloy documentation, especially `private-api.md` and `storage.md`.

## Key gotchas

- **`run_code` is a template, not a live script.** Placeholders `{{var}}` are string-replaced with actual values *before* execution. `inputs.get()` does not work — use placeholders directly. Quote strings (`name = "{{input.name}}"`), don't quote numbers/booleans (`count = {{input.count}}`).
- **`run_code` returns its last expression** (Jupyter-style) — do not use `return`. Put the result on the last line. For if/else, assign to a variable and put that variable on the last line.
- **`{{input.*}}` only resolves from the direct predecessor.** To use data from further back, save it to state.
- **Condition branches that merge** must share the input field they reference — it has to exist in every predecessor's output schema.
- **Child workflows do not erase the need for explicit data flow.** If a child needs parent data, pass it in payload or save it in `state` on purpose. If the parent needs the child's result later, save it to `state` after the child returns instead of assuming it will remain available several steps later.
- **Org workflows are `structured` type.** The structured palette cannot publish an Alloy AI (full agent) node; use `run_employee` for agent work. `mapping` and `wait_callback` step types exist in old type definitions but are not runnable — do not use them.
