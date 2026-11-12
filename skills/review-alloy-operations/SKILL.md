---
name: review-alloy-operations
description: Use when reviewing Alloy operational setup, teammates, instruction skills, workflows, MCP assignments, or recent activity over a time range; especially for org audits, configuration reviews, run-log analysis, and optimization recommendations.
---

# Review Alloy Operations

Use this skill together with `work-with-alloy`.

This skill is for operational reviews of an Alloy org: teammates, instruction skills, workflows, MCP setup, recent activity, run evidence, and optimization opportunities.

## When to Use

Use this when the user wants any of the following:

- an org audit of AI teammates, instruction skills, workflows, or MCPs
- a review of what happened over a date range
- a review of teammate or workflow configuration quality
- a review of run logs or operational patterns
- optimization recommendations for how the org is set up

Do not use this for normal one-off teammate creation, small workflow edits, or ordinary Storage note work.

## Required references

Before starting, load the relevant Alloy skills through MCP and read these system
references with `system_read_documentation`:

- `Team space/tech docs/references/teammates-and-workflows.md` for teammate configuration
- `Team space/tech docs/references/skills.md` when reviewing instruction skills
- the `workflows` skill for workflow design and execution
- `Team space/tech docs/references/multi-agent.md` when delegation is needed

Then read the bundled `references/operational-review-pattern.md`.

Use `references/review-note-template.md` for per-object notes.

## Scope gate

Before starting the review, determine the review scope:

- full org review
- subset review

If the user does not specify, ask which they want.

If subset review, clarify the minimum needed scope:

- which teammates, instruction skills, workflows, MCPs, or areas
- date range
- whether the focus is configuration, run evidence, optimization, or all of them

## Review flow

1. Confirm scope:
   - full org review or subset review
   - exact objects and date range when the scope is partial
2. Build the audit surface:
   - active AI teammates only
   - current org and teammate-assigned workflows
   - shared instruction skills under `.skills` when they are in scope
   - MCP servers
3. Exclude soft-deleted objects.
4. Treat workflow-list results only as candidates. Verify existence, readability, and ownership: `employee_id = null` means organization-owned; a populated `employee_id` means teammate-assigned. Reconcile with the UI when needed.
5. If the task is broad and subagents are available, fan out one subagent per teammate, workflow, or instruction skill in scope. Keep each subagent narrow and require it to write its note before returning.
6. Review runs selectively. Start with run lists, then inspect only failed, high-volume, suspicious, long-running, or representative successful runs.
7. Exclude clear internal testing from production conclusions, but still note real config or safety issues exposed by those runs.
8. Synthesize both:
   - per-object findings
   - cross-cutting findings

## Run-log routes

Use `system_do_organization_action` with:

- `GET /:organization_id/workflows/runs/`
- `GET /:organization_id/workflows/:workflow_id/runs/:run_id/`

## Deliverables

- Intermediary notes may stay local when the user asks for a local-first review flow.
- The final report should go to Alloy unless the user explicitly asks for another destination.
- Include prioritized fixes, not just observations.
