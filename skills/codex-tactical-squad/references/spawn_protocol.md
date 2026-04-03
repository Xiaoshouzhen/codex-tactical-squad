# Spawn Protocol

Use this protocol to enforce that the triad runs through actual subagent launches rather than main-thread role-play.

## Rule

If the triad is active, the main agent must explicitly launch subagents for planner, executor, and reviewer work.
Before launching a planner or executor, the main agent should first check whether a reusable tactical position subagent already exists for that role.

## Sequence

1. Build a task intake.
2. Check the tactical position registry or current session state for an existing planner subagent.
3. Reuse the planner if suitable; otherwise launch a planner subagent with the planner prompt and intake.
4. Receive the round contract from the planner subagent.
5. Check the tactical position registry or current session state for an existing executor subagent for this task cluster.
6. Reuse the executor if suitable; otherwise launch an executor subagent with the executor prompt and approved contract.
7. Receive the implementation report from the executor subagent.
8. Launch a fresh reviewer subagent with the reviewer prompt, task, contract, and executor report unless there is a specific reason to reuse the reviewer.
9. Receive the review report from the reviewer subagent.
10. Return the review report to the planner subagent for accept or rework.

At each launch or reuse step, inject the templates and task-local runtime context specified in `prompt_assembly.md`.
At each step, wait for the required structured submission before advancing to the next node.

## Prohibited Shortcut

Do not emulate the triad by having the main thread produce planner, executor, and reviewer sections in one response.
Do not spawn a new planner or executor by reflex if a suitable tactical position subagent is already active.

## Bypass

If explicit subagent launch is unavailable, declare bypass and explain why the triad cannot be fully activated.
