# Logs and Inboxes

## Ideas Inbox

Keep a lightweight inbox note at `Personal/<alloy_username>/future_ideas_inbox.md`.

When the user says something should be worked on in the future, add it there with the date and a short description.

If work on that topic is already happening, also reference the related folder or specific file. If it is still just a standalone idea, no reference is needed.

If the user passes a URL, open it, check what it is, and add a short comment.

## Artifact Log

Keep a lightweight artifact log at `Personal/<alloy_username>/.artifact-log/artifact-log.md` as a rough memory index of meaningful work artifacts. This includes work done in other systems at the user's request, such as creating a CRM record or updating a task manager card.

Each entry must be exactly one line:

```text
YYYY-MM-DD | /path/to/artifact-or-folder/ | One-sentence description.
```

Rules:

* Log meaningful artifacts, deliverables, notes, research outputs, specs, and folders of related outputs.
* Prefer the folder path over individual versioned files when several versions belong together.
* Do not log temp files, tiny intermediate files, or duplicate versions of the same artifact family.
* Append new entries to the bottom only. Do not sort or rewrite old entries.
* Keep using `artifact-log.md` until it reaches 1000 lines. Then rename it to `YYYY-MM-DD-artifact-log.md`, create a fresh empty `artifact-log.md`, and continue there.
* If unsure whether to log something, ask yourself: "Would future-us plausibly want to find this later?" If yes, log it.

## Review Log

Use:

* `Personal/<agent_name>/.review_log/failures/` - one file per significant case where you failed to help or led the user astray.
* `Personal/<agent_name>/.review_log/learnings/` - one file per significant case where the solution was achieved only after a long process that could have been much more efficient.

Rules:

* One file equals one case.
* Only write significant cases, not every conversation.
* File name: `YYYY-MM-DD_short-slug.md`.
* Keep entries short and useful for improving instructions, documentation, or search.

Entry template:

```md
Date:
Conversation / context:
What the user wanted:
What happened:
Why it happened: instructions | docs | search | ambiguity | no-tool | other
What should have happened:
What to improve:
Status: new
```
