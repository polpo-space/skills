# Jira CLI command reference

Reference for [`ankitpokhrel/jira-cli`](https://github.com/ankitpokhrel/jira-cli). Check `jira <command> --help` when installed CLI behavior differs from these examples.

## Connection and identity

```bash
command -v jira
jira init
jira me
jira serverinfo
```

## View issues

```bash
jira issue view PROJ-123
jira issue view PROJ-123 --comments 5
jira issue view PROJ-123 --raw
jira open PROJ-123
```

## List and search

```bash
# Current user's issues
jira issue list -a"$(jira me)"

# Common filters
jira issue list -s"In Progress"
jira issue list -tBug
jira issue list -yHigh
jira issue list -lurgent -lbug

# Combined filters
jira issue list -a"$(jira me)" -s"In Progress" -yHigh

# Text and dates
jira issue list "login error"
jira issue list --created week
jira issue list --updated -2d

# JQL
jira issue list -q"status = 'In Progress' AND assignee = currentUser()"

# Script-friendly output
jira issue list --plain --no-headers
jira issue list --plain --columns key,summary,status,assignee
jira issue list --paginate 20
```

## Create issues

Prefer interactive creation when project-required fields are unknown:

```bash
jira issue create
```

Non-interactive examples:

```bash
jira issue create \
  -pPROJ \
  -tBug \
  -s"Login button does not respond" \
  -b"Users cannot click the login button in Safari" \
  -yHigh \
  -lbug -lurgent

jira issue create -pPROJ -tTask -s"Update documentation" -a"$(jira me)"
jira issue create -pPROJ -tSub-task -P"PROJ-123" -s"Add regression coverage"
```

For a multi-line description, use a quoted heredoc so the shell does not expand its contents:

```bash
body_file="$(mktemp)"
cat >"$body_file" <<'EOF'
## Context
Describe the problem and why it matters.

## Acceptance criteria
- First observable outcome
- Second observable outcome
EOF

jira issue create --no-input \
  -pPROJ \
  -tStory \
  -s"Add export functionality" \
  -b"$(cat "$body_file")"

rm -f "$body_file"
```

Use `--no-input` only after confirming all required fields.

## Assign issues

```bash
jira issue assign PROJ-123 "$(jira me)"
jira issue assign PROJ-123 "user@example.com"
jira issue assign PROJ-123 default
jira issue assign PROJ-123 x
```

## Comments

```bash
jira issue comment add PROJ-123 -b"The fix has been deployed."
jira issue comment add PROJ-123 --template /path/to/comment.md
```

## Transitions

Always view the issue before moving it.

```bash
jira issue view PROJ-123
jira issue move PROJ-123 "In Progress"
jira issue move PROJ-123 "Done" --comment "Implementation completed"
jira issue move PROJ-123 "Done" -R"Fixed"
```

Workflow state and resolution names vary by Jira project. Use CLI help or interactive mode when the target transition is unclear.

## Sprints

```bash
jira sprint list
jira sprint list --state active
jira sprint add SPRINT-ID PROJ-123
jira sprint close SPRINT-ID
```

Closing a sprint is a high-impact operation and requires explicit confirmation.

## Link issues

```bash
jira issue link PROJ-123 PROJ-456 "Relates"
jira issue link PROJ-100 PROJ-200 "Blocks"
jira issue link PROJ-EPIC PROJ-STORY "Epic-Story"
```

Confirm the relationship direction before creating a `Blocks` link.

## Projects and boards

```bash
jira project list
jira board list
```

## Troubleshooting

```bash
# Reconfigure credentials/server
jira init

# Verify identity and connectivity
jira me
jira serverinfo

# Inspect exact command syntax for installed version
jira issue --help
jira issue create --help
jira issue list --help
```

Common failure classes:

| Error | Likely cause | Next step |
|---|---|---|
| Issue not found | Wrong key or inaccessible project | Verify key and permissions |
| Transition unavailable | Workflow constraint | Read current state and inspect valid transitions |
| Permission denied | Missing project permission | Ask a Jira administrator |
| Invalid assignee | Identifier not accepted | Use an exact email/account supported by the instance |
| Required field missing | Project-specific configuration | Use interactive creation or supply the required field |
