---
name: cursor-handoff
description: Hand the current conversation off to a fresh Cursor Agent CLI session managed by Herdr, with tmux as a fallback.
argument-hint: "What will the next Cursor session focus on?"
disable-model-invocation: true
---

Create a compact handoff summary of the current conversation, then immediately launch a fresh interactive Cursor Agent CLI session with that summary as its first prompt.

The new process must run in the current working directory and return control to the caller immediately. Prefer Herdr because it already owns the PTY, agent identity, status detection, and attach lifecycle. Use tmux only when no active Herdr server is available. Never nest tmux inside Herdr.

Do not merely print a launch command. Execute it.

## Build the handoff prompt

Choose a descriptive task name of 3-8 words. If the user passed arguments, treat them as the next session's focus and tailor both the task name and handoff prompt accordingly.

The prompt should contain only the context needed to continue:

```markdown
Task name: <descriptive task name>

## Focus
<what the next session should accomplish>

## Current state
<what has already happened and where the work stands>

## Decisions and constraints
<binding choices, exclusions, compatibility requirements, and user preferences>

## Existing artifacts
<paths or URLs to specs, issues, plans, ADRs, commits, diffs, or other source-of-truth artifacts>

## Remaining work
<concrete next actions and unresolved questions>

## Validation
<checks already run and checks still required>

## Suggested skills
<skills the Cursor agent should invoke, with a short reason for each>
```

Do not duplicate material already captured in another artifact. Reference the artifact by path or URL and state why it matters.

Do not paste the conversation transcript. Summarize decisions and operational state.

Redact secrets and personally identifiable information, including API keys, passwords, access tokens, private keys, email addresses, phone numbers, user IDs, and precise personal locations. The summary becomes the new agent's prompt.

## Preflight

1. Resolve the workspace with `pwd -P`.
2. Resolve the Cursor binary, preferring `agent` and falling back to `cursor-agent`.
3. Run `<cursor-binary> status` and stop if Cursor is not authenticated.
4. Require either:
   - `herdr` with a reachable server, checked with `herdr api snapshot >/dev/null 2>&1`, or
   - `tmux` as the fallback.

If no Cursor binary exists, stop and report that Cursor Agent CLI must be installed. If neither Herdr nor tmux is usable, stop rather than pretending the handoff launched.

## Prepare a private launcher

Write the handoff prompt to a temporary file outside the workspace with permissions limited to the current user. Create a temporary executable launcher that:

1. reads the entire prompt from the temporary handoff file,
2. removes both temporary files,
3. replaces itself with the Cursor process using:

```bash
exec "$cursor_bin" --force "$prompt"
```

Use interactive mode. Do not use `--print`, because the session must remain attachable for follow-up work. Do not use Cursor's `--background`; Herdr or tmux owns the background lifecycle.

Do not interpolate the handoff text directly into a shell command. Pass it through the private temporary file so multiline text and shell metacharacters cannot alter the launch command.

## Preferred launch: Herdr

When `herdr api snapshot >/dev/null 2>&1` succeeds, launch the temporary wrapper as a named Herdr agent:

```bash
herdr agent start "<descriptive unique name>" \
  --cwd "$workspace" \
  --no-focus \
  -- "$launcher"
```

Use a readable name derived from the task name and append a short timestamp when needed to avoid collisions. `--no-focus` is required so the handoff starts in the background without stealing the current pane.

After launch, verify it appears in `herdr agent list`. Report these management commands to the user:

```bash
herdr agent focus "<name>"
herdr agent attach "<name>" --takeover
herdr agent read "<name>" --source recent --lines 200
herdr agent send "<name>" "<follow-up instruction>"
```

## Fallback launch: tmux

When Herdr is unavailable or its server is not reachable, launch the same temporary wrapper in a detached tmux session:

```bash
tmux new-session -d \
  -s "<unique-shell-safe-session-name>" \
  -c "$workspace" \
  "$launcher"
```

Use a lowercase, shell-safe session name derived from the task name plus a short timestamp. Verify it appears in `tmux list-sessions`. Report these management commands to the user:

```bash
tmux attach -t "<session-name>"
tmux capture-pane -p -t "<session-name>" -S -200
tmux kill-session -t "<session-name>"
```

## Completion response

Report:

- the selected backend (`herdr` or `tmux`),
- the agent or session name,
- the workspace path,
- the attach/focus command,
- any immediate blocked state, such as a Cursor workspace-trust prompt.

Do not claim success unless the Herdr agent or tmux session is visible after launch.
