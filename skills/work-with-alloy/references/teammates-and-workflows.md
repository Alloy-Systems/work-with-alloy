# AI Teammates and Workflows

## Add a new AI teammate

If the user did not provide information about the new teammate's role, ask about it. Otherwise proceed without extra questions.

If the user did not provide a name, create a short appropriate name. It can be lightly playful.

Read canonical teammate profiles in Alloy documentation path `/tech docs/Teammate profiles` and use a good starting point if one exists. If no profile exists, add one from scratch.

Leave the LLM empty unless the user asks for a specific model, so the teammate uses the organization default. Leave voice fields unset unless voice is needed; when it is, select TTS, STT, and voice values from the models currently available to the organization instead of hard-coding model IDs.

Storage write access requires read access, so add both when the user asks for write access.

## Tweak AI teammate instructions

If the teammate's instructions become longer than about 800 characters, move them into a separate Markdown note in Alloy Storage and reference that note from the instructions. Share the containing folder with the teammate so it can read the note.

After changing the instructions, stress test them with the teammate. Check instruction following and likely edge cases.

Advise on an LLM that fits the instructions, based on general model knowledge.

## Add a webchat widget

When adding a webchat widget to a teammate, post the widget script in chat so the user can test it.

If the user asks how to integrate it into their system, give instructions but explain that you cannot directly install it in their system. You can suggest adding or using a teammate with `run_code` and asking it to deploy the widget through an API when the user's CMS supports that.

## Copy an existing teammate

Do not ask extra questions. Make an exact copy of the existing teammate with all workflows, channels, storage access, and related settings.

After creating the copy, compare it with the original. Provide its link, then ask whether the user wants to share storage folders with the copied teammate.

## Add a workflow for a teammate

A teammate workflow uses the same engine and builder as an organization-owned workflow. For step-graph design and choosing deterministic, LLM, or agent steps, see `references/workflows.md`. The points below are teammate-specific.

If a built-in tool can perform the requested function, suggest adding that tool instead. `run_code` and HTTP request tools are exceptions: explain their security risks and get confirmation before connecting them.

Check Alloy documentation for existing blueprints and use them when available.

Suggest workflow inputs and outputs and ask for confirmation.

If the workflow requires an API key, ask the user to add a variable. Tell them not to post the key in chat.

Never use `send_message` or `user_input` nodes.

Build workflows to fail safely. If something unexpected happens, return an explanation to the teammate describing what happened and how to fix it.

Add a workflow description that explains how to use it. The description and input schema are what the teammate sees when deciding whether to call the workflow.

For structured workflows, cap output provided to the teammate. Unless instructed otherwise, use a 10,000-character total output cap.

If the workflow accepts arbitrary text, handle quotes, JSON, and multiline content safely.

After building a workflow, test it immediately. If the test fails, fix it and test again. Return to the user only when it works or when you need an action from them.

For reusable instruction packages rather than executable graphs, use `references/skills.md`.
