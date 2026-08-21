# Operational review pattern

## Goal

Review Alloy org setup and recent operations without overwhelming coordinator context.

Be thorough. No rush.

## Pattern

1. Scope gate
   - Decide whether this is:
     - a full org review
     - a subset review
   - If the user does not specify, ask.
   - For subset reviews, pin down:
     - exact teammates/instruction skills/workflows/MCPs/areas
     - date range
     - whether the focus is configuration, run evidence, optimization, or all of them

2. Coordinator inventory and topology
   - Read references related to instruction skills, workflows, and AI teammates. Use them as best-practice baselines.
   - Collect the full inventory of:
     - active AI teammates only
     - current organization-owned and teammate-assigned workflows
     - shared instruction skills under `.skills` when they are in scope
     - MCP servers
   - Build a topology view:
     - which teammate owns which workflow(s)
     - which teammates can read each instruction skill
     - which MCPs are assigned to which teammates
     - which workflows are org-level vs teammate-level
     - which objects were active in the target date range
   - Exclude soft-deleted objects from the audit surface:
     - teammates: explicitly check the soft-deleted teammate list and keep them out
     - workflows: treat list results only as candidates, then verify existence, readability, and ownership; `employee_id = null` means organization-owned and a populated `employee_id` means teammate-assigned
     - instruction skills: verify the `.skills/<name>/SKILL.md` package, supporting files, and effective teammate read access

3. One subagent per object
   - Spawn one subagent per AI teammate.
   - Spawn one subagent per workflow.
   - Spawn one subagent per instruction skill when skills are in scope.
   - Each subagent stays narrow and reviews only its assigned object.
   - Make them read relevant reference notes when needed.
   - Require every subagent to write its findings into the shared review folder before returning.

4. Per-object review contract
   - Identity:
     - name
     - id
     - owner/scope
   - Configuration:
     - prompt/setup
     - attached workflows
     - shared instruction skills when relevant
     - workflow graph / step structure
     - triggers / schedules / debounce / mode
     - MCP and tool access
     - for instruction skills: `SKILL.md` front matter, instructions, references, scripts, assets, and teammate access
   - Operational evidence:
     - run list during target date range
     - failures
     - duplicate or suspicious runs
     - high-volume patterns
     - notable outputs or missing outputs
   - Problems:
     - config bugs
     - prompt or routing weaknesses
     - tooling mismatches
     - access issues
     - observability gaps
   - Suggestions:
     - concrete optimizations
     - priority
     - expected impact

5. Run-log discipline
   - Do not load every run detail.
   - First fetch run lists.
   - Then inspect only selected runs:
     - failed runs
     - high-volume runs
     - long or suspicious runs
     - representative successful runs
   - Use `system_do_organization_action` with:
     - `GET /:organization_id/workflows/runs/`
     - `GET /:organization_id/workflows/:workflow_id/runs/:run_id/`

6. Test-run exclusion
   - Ignore runs that are clearly internal testing or feature validation by the Alloy team.
   - When uncertain, mark the run as ambiguous instead of treating it as production evidence.
   - Keep soft-deleted teammates and workflows out of conclusions even if their old runs appear in historical logs.

7. Coordinator synthesis
   - Merge subagent outputs into:
     - per-object findings
     - cross-cutting findings
     - repeated MCP issues
     - overlapping teammate/workflow responsibilities
     - systemic run-pattern issues
     - prioritized optimization plan

8. Deliverables
   - Intermediary notes can stay local if the user asks for that flow.
   - Final note goes to Alloy unless the user explicitly asks for another destination.

## Safeguards

- Use a fixed template for every subagent so results are comparable.
- Keep raw logs inside subagents as much as possible.
- Coordinator should read summaries first and raw logs only for spot checks.
- Cross-object issues must be synthesized centrally; they will not emerge reliably from isolated object reviews.

## Execution order

1. Inventory and topology
2. Subagent fan-out
3. Partial result collection
4. Cross-object synthesis
5. Spot-check raw logs where needed
6. Final recommendations
