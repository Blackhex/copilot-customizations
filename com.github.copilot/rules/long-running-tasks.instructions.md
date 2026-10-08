---
applyTo: "**"
description: "Recover long-running, background, or ambiguous terminal commands and finish the original task."
---

# Finish long-running tasks

## Run

- Use synchronous/foreground execution for one-shot builds, tests, installs, downloads, and scripts; avoid short timeouts. Reserve background mode for persistent servers, watchers, and daemons.
- Keep checks bounded and efficient. Before launching a potentially long, silent command, arrange independently inspectable progress logs and a completion receipt with exit status when supported. Capture the execution ID or PID as soon as available; identify an independent read-only recovery route if terminal tracking is unavailable.

## Track

- Treat detached or timed-out commands as pending. If completion is unverified and output, exit status, or execution ID is missing, treat execution as unknown: the command may still be running. Neither state alone proves success, failure, or a blocker.
- Record the goal, state, available terminal/execution ID or PID, logs, expected result, and next action. Keep this handoff available across context compaction; do not invent missing identifiers.

## Recover

- Do not send unrelated commands to a busy or unknown terminal, duplicate a potentially active command, or cancel user/shared processes.
- Resolve unknown state through supported completion tracking or independent read-only inspection of the known process and retained logs/receipts. If the preferred runner cannot inspect it, try another safe independent execution context, such as an agent-owned terminal or read-only inspection task, before declaring a blocker. One runner's limitation does not prove that independent inspection is unavailable. Silence, partial output, or log timestamps alone do not establish completion.
- Repair ordinary failures or inefficient checks within scope, then rerun required validation. Cancel or restart only a verified agent-owned command when safe and authorized. Apply the same verification before asking the user to interrupt it, including confirming that the matching process is still running; missing output alone is insufficient. Preserve user work and never weaken validation.

## Delegated work

- A subagent reply that mentions a pending, background, or still-running command is not a completion. Adopt the command as your own pending work: record its terminal ID, log path, and expected result.
- Do not end the turn while adopted work is pending and the next step depends on it. Wait in the foreground with a bounded command that exits when the process ends or the log shows its summary line, prints a heartbeat line at least every 30 seconds so the tool does not detach it, and has a maximum duration. This bounded wait is the only allowed use of sleep.
- When dispatching a subagent for long checks, require it to hold its reply until the final summary line exists, and verify that line yourself before accepting the result.
- Never end a turn with "when it finishes I will continue": nothing invokes an ended turn.

## Resume

- On the next invocation, including completion notifications or status questions, reconcile the handoff first. Respect newer instructions to pause, cancel, or redirect the task.
- Retrieve final output and exit status, verify expected results/receipts, then continue the original task without requiring another "continue" message. A complete synchronous result needs no background-output lookup.
- Follow the tool's waiting rules; do not poll in a loop of separate tool calls. If the host forces ending the turn (not merely because a wait is inconvenient), report the pending/unknown command and next action, not completion. Continue until resolved, a verified safety/external blocker remains after reasonable recovery, or an explicit workflow stop condition applies.

Instructions guide continuation when invoked; they cannot force VS Code to deliver notifications, expose missing process handles, or wake a stopped agent.