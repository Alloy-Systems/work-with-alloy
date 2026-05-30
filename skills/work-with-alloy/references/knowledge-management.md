# Knowledge Management

## Canonical knowledge locations

* **Organization**: `Organization/knowledge.md` and linked notes. Company-wide facts, policies, source-of-truth pointers, cross-team rules, and shared caveats.
* **User**: `Personal/<alloy_username>/.knowledge/knowledge.md` and linked notes. Durable facts about a person: role, preferences, working style, stable constraints, and recurring context.
* **Area**: `<area>/.knowledge/knowledge.md` and linked notes. For a customer, partner, product area, project, department, or folder — including sources of information, how things are done, how decisions are made, do/don't rules, escalation contacts, and notification expectations.
* **Agent**: `Personal/<agent>/.knowledge/knowledge.md` and linked notes. Only for knowledge about a specific agent's role or behavior that does not fit elsewhere.

## When and where to write

Update knowledge when the user explicitly asks, or when you notice durable/reusable knowledge during normal work. Do not save one-off details, obvious facts, or information already clear from nearby documents, tickets, code, or source systems.

Put knowledge in the most specific relevant place. Default to area-level knowledge. Ask yourself why it is not an area note, and whether it is truly only the user's individual preference or a best practice everyone should use. If unsure, ask the user.

Create missing area and `.knowledge/` folders when needed.

You may create, update, and delete stale/deprecated knowledge without asking.

If user input, local knowledge, product docs, CRM, or another system disagree, stop and ask the human to clarify.

Before relying on a knowledge note that names a file, function, setting, source, or external system, verify that the referenced thing still exists when practical.

## Note format

Use one file for one small fact, instruction, caveat, reference, or reusable decision principle. Keep individual knowledge notes under about 1000 characters; split or trim if larger.

After creating or updating a note, update the relevant `knowledge.md` directory note.

Keep `knowledge.md` mostly navigational: overview around 300 characters max, total around 1000 characters max.

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
Type: preference | feedback | reference | source | playbook | caveat | decision_rule
(Type = content nature; where the file lives determines scope — org/user/area/agent)

<One compact fact, instruction, caveat, reference, or reusable rule.>

How to apply: <How an agent should use this later. Omit if obvious.>
Source: <person, document, system, ticket, thread, URL, or file path>
```
