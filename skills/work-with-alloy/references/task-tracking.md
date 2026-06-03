# Task Tracking

Read this before creating or updating a task in the org task tracker. The trigger and decision boundary live inline in SKILL.md ("Task tracking"); the mechanics are here.

## Which tracker

The org's task tracker may be Linear, Jira, or a homegrown system, reached through the connected `mcp__alloy__<System>__*` tools.

**If you don't already know which tracker the org uses, ask the user once**, then persist the answer for future sessions.

If no tracker is connected via Alloy MCP, flag it to the user to connect in Alloy settings rather than wiring up a direct integration yourself.

## Lifecycle

Create the task **when you start** the work (status Todo / In Progress, assigned to the user unless told otherwise), update it as it progresses, and **mark it Done when finished**.

Follow the org's existing conventions — before creating the first task in a session, look at recent issues in the tracker to match team/project, assignee, status names, and title style.

## Follow-ups update the existing task

Don't leave a finished task stale. If the user asks for more work on something that already has a task, find that task and work through it rather than spawning a duplicate. When the task was already Done, move it back to In Progress (or the tracker's equivalent) while you work, append what changed, then set it back to Done when complete.

Open a *new* task only when the follow-on is substantial and independent enough to stand on its own.

## Is it a task?

| Request | Task? | Why |
|---------|-------|-----|
| "How do I deploy the site?" | No | Ephemeral question, answered in chat |
| "Research X and just tell me" (no saved note) | No | Chat-only answer, no durable artifact |
| "Create a deck for prospect Y" | Yes | Produces a deliverable |
| "Write a playbook on Z" | Yes | Produces a durable artifact |
| "Research X **and write a note**" | Yes | The note is the deliverable |
| "Fix the counter bug" | Yes | Changes a deliverable we own (code) |
| "Create a lead in Attio" | No | One-shot mutation of an external system |
| "Send this email" | No | One-shot external action |

When genuinely unsure whether a request is a task, ask.
