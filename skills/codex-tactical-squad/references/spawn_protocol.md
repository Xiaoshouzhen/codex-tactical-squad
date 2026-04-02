# Spawn Protocol

Use this protocol to enforce that the triad runs through actual subagent launches rather than main-thread role-play.

## Rule

If the triad is active, the main agent must explicitly launch subagents for planner, executor, and reviewer work.

## Sequence

1. Build a task intake.
2. Spawn a planner subagent with the planner prompt and intake.
3. Receive the round contract from the planner subagent.
4. Spawn an executor subagent with the executor prompt and approved contract.
5. Receive the implementation report from the executor subagent.
6. Spawn a reviewer subagent with the reviewer prompt, task, contract, and executor report.
7. Receive the review report from the reviewer subagent.
8. Return the review report to the planner subagent for accept or rework.

## Prohibited Shortcut

Do not emulate the triad by having the main thread produce planner, executor, and reviewer sections in one response.

## Bypass

If explicit subagent launch is unavailable, declare bypass and explain why the triad cannot be fully activated.
