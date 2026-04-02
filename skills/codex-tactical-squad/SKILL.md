---
name: codex-tactical-squad
description: Run coding tasks through a three-subagent planner, executor, reviewer loop. Use when Codex is asked to write, modify, refactor, or fix code and should follow a repeatable plan-execute-review workflow with explicit subagent launches, a classic scalpel architecture, or a closed-loop coding harness.
---

# Codex Tactical Squad

Use this skill for coding tasks that should go through a planner, executor, reviewer loop.
Activate the triad by explicitly launching subagents for those roles rather than role-playing them in the main thread.

## Roles

1. `Planner`: create the round contract, surface blocking ambiguities, and make the accept or rework decision after review.
2. `Executor`: implement only the approved write scope and report exact changed paths plus verification.
3. `Reviewer`: inspect the result against the task, contract, and executor report.

## Required Artifacts

Use these bundled references:

- [task_intake_template.md](references/task_intake_template.md)
- [round_contract_template.md](references/round_contract_template.md)
- [implementation_report_template.md](references/implementation_report_template.md)
- [review_report_template.md](references/review_report_template.md)
- [planner_decision_template.md](references/planner_decision_template.md)
- [replan_request_template.md](references/replan_request_template.md)
- [orchestrator_checklist.md](references/orchestrator_checklist.md)
- [spawn_protocol.md](references/spawn_protocol.md)

## Rules

- Start with a task intake and planner contract before code edits.
- If the triad is active, explicitly spawn subagents for planner, executor, and reviewer work.
- Treat write scope as exact file paths plus explicitly allowed adjacent edits.
- If blocking ambiguities remain, the planner should stop and surface them instead of guessing.
- If the executor cannot finish without scope expansion or assumption changes, return a replan request instead of improvising.
- The reviewer should report findings first, ordered by severity, and use the executor report as input.
- The planner should not accept a round if the reviewer found material issues unless the residual risk is explicitly accepted.

## Bypass

Bypass the full triad only for non-coding tasks, trivial tasks where the loop would be pure overhead, or when subagent tools or explicit subagent launch are unavailable. State the bypass reason explicitly.
