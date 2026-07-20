---
name: jira
description: Use the jira CLI when the user mentions Jira issues such as PROJ-123, asks about tickets, wants to create, view, update, assign, comment on, or transition issues, or asks about sprints, backlogs, and JQL searches.
---

# Jira CLI

Use the locally installed [`jira` CLI](https://github.com/ankitpokhrel/jira-cli) to interact with Jira in natural language.

This skill uses only the local CLI. Do not look for or substitute another backend.

## Availability and environment check

Run this before the first Jira operation in a conversation:

```bash
which jira
```

If the command is unavailable, stop and guide the user through setup:

```bash
brew install ankitpokhrel/jira-cli/jira-cli
jira init
```

Do not silently substitute another Jira backend.

All Jira CLI commands must load the user's Jira environment file in the same shell:

```bash
source ~/.jira-cli.env
```

The environment file is expected to provide authentication settings, for example:

```bash
export JIRA_API_TOKEN="..."
export JIRA_AUTH_TYPE="bearer"
```

Run every Jira command like this:

```bash
source ~/.jira-cli.env && jira issue view PROJ-123
```

**Hard rule:** Before every Jira CLI invocation, source `~/.jira-cli.env` in the same shell. Never assume the current shell has already loaded these variables.

## Quick reference

| Intent | Command |
|---|---|
| View issue | `jira issue view ISSUE-KEY` |
| List my issues | `jira issue list -a"$(jira me)"` |
| My in-progress issues | `jira issue list -a"$(jira me)" -s"In Progress"` |
| Create issue | `jira issue create -tType -s"Summary" -b"Description"` |
| Transition issue | `jira issue move ISSUE-KEY "State"` |
| Assign to me | `jira issue assign ISSUE-KEY "$(jira me)"` |
| Unassign | `jira issue assign ISSUE-KEY x` |
| Add comment | `jira issue comment add ISSUE-KEY -b"Comment text"` |
| Open in browser | `jira open ISSUE-KEY` |
| List active sprints | `jira sprint list --state active` |
| Current user | `jira me` |

Issue keys normally match `[A-Z][A-Z0-9_]*-[0-9]+`, for example `PROJ-123`.

## Read operations

Execute read-only operations directly unless the user asks to see the command first.

Examples:

- View an issue and its current status.
- List issues assigned to the current user.
- Search with filters or JQL.
- List projects, boards, or active sprints.

Use `references/commands.md` for complex filters, JQL, pagination, and raw output.

## Write operations

Creating, editing, assigning, commenting, linking, moving, or bulk-changing issues modifies shared Jira state.

Before a write:

1. Fetch the current issue when one already exists.
2. Identify the exact requested change.
3. Show the proposed change and command.
4. Get explicit user approval unless the user's current message directly and unambiguously instructs you to perform that exact change.
5. Execute the command.
6. Fetch the issue again or otherwise verify the result.

For bulk changes, always obtain explicit approval after showing the affected issue keys, even when the initial request was broad.

## Safety rules

- Never transition an issue without first reading its current state.
- Never assume workflow state names are universal across projects.
- Never replace a description without showing or preserving the existing description.
- Never use `--no-input` unless all required fields are known and supplied.
- Never bulk-modify issues without listing the targets and obtaining explicit approval.
- Prefer issue keys and exact identifiers over ambiguous summaries or display names.
- Surface authentication, permission, and required-field errors without trying destructive workarounds.

## Ticket creation workflow

When the user asks to create a ticket:

1. Gather relevant context from the conversation and referenced code or documents.
2. Search Jira for an obvious duplicate when practical.
3. Draft a concise summary and structured description.
4. Confirm project, issue type, and any required fields that are not inferable.
5. Create the issue using the CLI.
6. Return the created issue key and verify its fields.

For multi-line descriptions, write the body to a temporary Markdown file or use a quoted heredoc, then pass the content safely. See `references/commands.md`.

## Load the command reference when

- Creating an issue with multi-line content or custom fields.
- Building non-trivial JQL.
- Transitioning, linking, assigning, or working with sprints.
- Troubleshooting authentication or CLI errors.

Simple issue views and basic lists can use the quick reference without loading the full file.

## Attribution

Adapted from the Jira skill in `softaworks/agent-toolkit`, retaining only its CLI workflow and adjusting it for this repository.
