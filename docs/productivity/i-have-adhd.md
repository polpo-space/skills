## What it does

`i-have-adhd` reshapes every reply so an ADHD brain can act on it: the next action comes first, multi-step work is numbered, state is restated each turn, and tangents stay suppressed until the current job is done.

It is not "be brief." Brevity without a clear next action still loses the thread. The defining constraint is **action-first persistence** — once invoked, the shape holds for the rest of the session until you say `stop adhd mode` or `normal mode`.

## When to reach for it

You invoke this by typing `/i-have-adhd` — the agent won't reach for it on its own.

Reach for it when you want answers you can execute immediately instead of buried plans, preamble, or "hope this helps" closers. Turn it off when you want the default style back. For writing or editing documents that agents consume, use [writing-for-agents](https://aihero.dev/skills-writing-for-agents); for compacting a thread so another agent can continue, use [handoff](https://aihero.dev/skills-handoff).

## Action first

The leading idea is **action first**: the first line is something you can do (a command, path, or snippet), not context. Numbered steps stay short; wins and time estimates stay concrete; lists cap at five or split into now/later.

It also forces a pre-send cut: delete announcing openers, recap closers, and figurative filler so the first and last lines alone tell you what just happened and what to do next.

## It's working if

- Replies start with a concrete next action, not "Great question" or "Let me…".
- Multi-step work appears as a short numbered list, one bounded action per step.
- Each turn restates where you are (e.g. "Step 3 of 5 done… Next: …").
- The mode stays on across topic changes until you explicitly stop it.

## Where it fits

`i-have-adhd` is a reach-for-it-anytime standalone — an output-style overlay, not a step in the grill → spec → tickets → implement chain. Its closest neighbours are [handoff](https://aihero.dev/skills-handoff) (cross-session continuity) and [writing-for-agents](https://aihero.dev/skills-writing-for-agents) (how documents for agents should read). For the whole map, see [ask-matt](https://aihero.dev/skills-ask-matt).
