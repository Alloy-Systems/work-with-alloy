# Skills

Use a skill when an AI teammate needs reusable instructions, domain guidance, or a repeatable method rather than an executable workflow graph.

## Create a skill

Create the skill in organization Storage at `.skills/<skill-name>/SKILL.md`. Use a lowercase hyphenated name and make the directory name match the `name` in front matter.

```markdown
---
name: ticket-summary
description: Use when an AI teammate needs to turn a support transcript into a title, summary, and urgency rating.
---

# Ticket summary

Read the complete transcript, then return a concise title, summary, and urgency.
```

The front matter requires `name` and `description`. The Markdown body contains the instructions. Put optional detailed material in `references/`, executable helpers in `scripts/`, and reusable resources in `assets/`.

Use Storage operations to create and update these files. Do not create a workflow entity or step graph for a skill.

## Make a skill available

Share `.skills/<skill-name>` with an AI teammate using read access. Share `.skills` only when the teammate should see every skill below it. Write access is needed only when the teammate should maintain the skill.

Do not place secrets in skill files. Use organization variables or connector configuration.

## Test a skill

Ask the intended teammate to perform a representative task that should trigger the skill. Confirm that it follows the skill and produces the expected result. If it does not, improve the description or instructions and test again.

Use `references/workflows.md` when the requested capability needs triggers, a step graph, workflow runs, or deterministic execution.
