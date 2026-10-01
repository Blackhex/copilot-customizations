---
applyTo: "**"
description: "Recover long-running, background, or ambiguous terminal commands and finish the original task."
---

# Finish long-running tasks

## Run

- Use synchronous/foreground execution for one-shot builds, tests, installs, downloads, and scripts; avoid short timeouts. Reserve background mode for persistent servers, watchers, and daemons.
- Keep checks bounded and efficient. For potentially long, silent commands, retain progress logs and an explicit completion receipt with exit status when supported.

## Track

- Treat detached or timed-out commands as pending. If completion is unverified and output, exit status, or execution ID is missing, treat execution as unknown: the command may still be running. Neither state alone proves success, failure, or a blocker.
- Record the goal, state, available terminal/execution ID or PID, logs, expected result, and next action. Keep this handoff available across context compaction; do not invent missing identifiers.

## Recover

- Do not send unrelated commands to a busy or unknown terminal, duplicate a potentially active command, or cancel user/shared processes.
- Resolve unknown state through supported completion tracking or independent read-only inspection of the known process and retained logs/receipts. Silence, partial output, or log timestamps alone do not establish completion.
- Repair ordinary failures or inefficient checks within scope, then rerun required validation. Cancel or restart only a verified agent-owned command when safe and authorized; preserve user work and never weaken validation.

## Resume

- On the next invocation, including completion notifications or status questions, reconcile the handoff first. Respect newer instructions to pause, cancel, or redirect the task.
- Retrieve final output and exit status, verify expected results/receipts, then continue the original task without requiring another "continue" message. A complete synchronous result needs no background-output lookup.
- Follow the tool's waiting rules; do not poll or sleep to wait. If the host requires ending the turn, report the pending/unknown command and next action, not completion. Continue until resolved, a verified safety/external blocker remains after reasonable recovery, or an explicit workflow stop condition applies.

Instructions guide continuation when invoked; they cannot force VS Code to deliver notifications, expose missing process handles, or wake a stopped agent.