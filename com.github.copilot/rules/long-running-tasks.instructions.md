---
applyTo: "**"
description: "Recover long-running, background, or ambiguous terminal commands and finish the original task."
---

# Finish long-running tasks

## Run

- Use synchronous/foreground execution for one-shot builds, tests, installs, downloads, and scripts; avoid short timeouts. Reserve background mode for persistent servers, watchers, and daemons.
- Before relying on automatic completion notifications, verify command detection for that terminal; an enabled setting, terminal ID or live PID is insufficient. If shell integration is missing or execution is idle-only, restore it in a fresh terminal when safe and authorized, or use supported foreground process/receipt tracking. Do not assume a callback.
- Keep checks bounded and efficient. Before launching a potentially long, silent command, arrange independently inspectable progress logs and a completion receipt with exit status when supported. Capture the execution ID or PID as soon as available; identify an independent read-only recovery route if terminal tracking is unavailable.

## Track

- Treat detached or timed-out commands as pending. If completion is unverified and output, exit status, or execution ID is missing, treat execution as unknown: the command may still be running. Neither state alone proves success, failure, or a blocker.
- A successful tool invocation or returned output snapshot is not a successful process exit. Even a synchronous call can return while its worker or foreground completion observer is still active; a tool-response time limit is not a forced end of the chat turn.
- Record the goal, state, available terminal/execution ID or PID, logs, expected result, and next action. Keep this handoff available across context compaction; do not invent missing identifiers.
- Report process liveness, work progress, and accepted completion as separate facts. A verified live PID means only that the process exists; it does not establish that the task is healthy, making progress, or producing valid work. For status updates, state the last verified stage and its age, completed-and-validated count versus planned count, active workload when known, failure/blocker count, and whether the completion receipt exists. Do not answer "still running" without saying what evidence verifies liveness and what remains unvalidated.
- For multi-unit jobs, inspect each durable unit result and its diagnostics as it closes, not only the final process exit. Treat explicit missing prerequisites, invalid outputs, or hard-stop diagnostics as blockers even when the worker stays alive or reports a zero exit. Preserve the evidence, stop only a verified owned worker when continuing would repeat known-invalid work, and report unstarted units as incomplete rather than successful or merely pending.

## Recover

- Do not send unrelated commands to a busy or unknown terminal, duplicate a potentially active command, or cancel user/shared processes.
- Resolve unknown state through supported completion tracking or independent read-only inspection of the known process and retained logs/receipts. If the preferred runner cannot inspect it, try another safe independent execution context, such as an agent-owned terminal or read-only inspection task, before declaring a blocker. One runner's limitation does not prove that independent inspection is unavailable. Silence, partial output, or log timestamps alone do not establish completion.
- Missing terminal execution IDs do not remove an available recovery route through a known task ID, verified process identity, or completion receipt. If a wait returns interim output, reconnect to that same task or live observer using supported blocking output retrieval. Do not start another observer or send more commands to its busy terminal.
- Repair ordinary failures or inefficient checks within scope, then rerun required validation. Cancel or restart only a verified agent-owned command when safe and authorized. Apply the same verification before asking the user to interrupt it, including confirming that the matching process is still running; missing output alone is insufficient. Preserve user work and never weaken validation.

## Pending work

- These continuation rules apply to all required work: directly launched commands, VS Code tasks, completion observers, and adopted subagent work. Do not end the turn while that work is pending and a supported continuation route remains available.
- Wait in the foreground with a bounded command that exits on process completion or the retained summary line, prints a heartbeat at least every 30 seconds, and has a maximum duration. Heartbeats expose progress; they do not guarantee that a tool stays attached. This bounded wait is the only allowed use of sleep.
- If the host caps one blocking retrieval and returns a running snapshot, continue retrieving the same pending task or observer in bounded blocking segments when the tool supports it. This is continuation of one wait, not permission for rapid status polling, duplicate workers, or overriding a tool's explicit waiting rules.
- Before finalizing with required work unfinished, identify the actual stop condition: a user pause/cancel, an explicit tool rule requiring return while awaiting a notification, a recorded host cancellation/turn limit, or a verified safety/external blocker after reasonable recovery. Interim output, an absent callback, elapsed time, or a missing terminal ID alone does not qualify.
- A tool-mandated return is still binding when notifications are unavailable. Use any supported independent foreground continuation without violating that rule; if none exists, report the missing capability and pending work rather than claiming a notification will resume the task.
- An automatic report writer, stored handoff, or background observer can preserve results but cannot resume an ended assistant turn. Do not substitute them for verifying and delivering the requested result, and never end with "when it finishes I will continue."

## Delegated work

- A subagent reply that mentions a pending, background, or still-running command is not a completion. Adopt the command as your own pending work: record its terminal ID, log path, and expected result.
- When dispatching a subagent for long checks, require it to hold its reply until the final summary line exists, and verify that line yourself before accepting the result.

## Resume

- On the next invocation, including completion notifications or status questions, reconcile the handoff first. Respect newer instructions to pause, cancel, or redirect the task.
- Retrieve final output and exit status, verify expected results/receipts, then finish the original task's analysis, validation, cleanup, and user-facing report without requiring another "continue" message. A complete synchronous result needs no background-output lookup.
- Follow the tool's waiting rules; do not rapidly poll status in a loop of separate calls. Supported bounded blocking retrieval of the same pending work remains allowed. If a verified stop condition forces ending the turn, report the pending/unknown command, evidence for that stop, and next action, not completion or a promised automatic resume. Continue until the requested deliverable is verified and presented, a verified safety/external blocker remains after reasonable recovery, or an explicit workflow stop condition applies.

Instructions guide continuation when invoked; they cannot force VS Code to deliver notifications, expose missing process handles, or wake a stopped agent.