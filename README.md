# goal-mode

A [pi coding agent](https://github.com/badlogic/pi-mono) extension that adds long-running autonomous goals — adapted from [Codex CLI's `/goal` feature](https://github.com/openai/codex/releases/tag/rust-v0.128.0).

## Install

```bash
cp -r goal-mode ~/.pi/agent/extensions/
```

Then `/reload` in pi, or restart.

## Usage

```
/goal add unit tests for all public functions
```

Then send any prompt to kick things off. The agent will keep working across turns until the goal is complete, the token budget runs out, or you pause/clear it.

```
/goal           — see current goal, budget, time spent
/goal pause     — pause the loop
/goal resume    — restart a paused goal
/goal clear     — delete the goal
```

## How it works

After every turn where the goal is active, a hidden continuation prompt is injected that tells the agent to keep working and run a completion audit before declaring done. The agent has three tools to interact with the goal:

- `create_goal` — start a new goal (fails if one exists)
- `get_goal` — read status, budget, token/time usage
- `update_goal` — mark complete (the model cannot pause/resume/budget-limit)

Goal state persists in pi's session file, so it survives restarts and works correctly with pi's branching/forking.