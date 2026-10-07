---
applyTo: "**"
description: "Create Git worktrees only when explicitly requested by the user."
---

# Require explicit worktree requests

- Do not create Git worktrees, including through native tools, scripts, or subagents, unless the user explicitly requests one for the current task.
- Work in the current checkout by default. A task, skill, or workflow recommending isolation is not authorization to create a worktree.