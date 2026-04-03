---
name: codex-tactical-squad
description: Tactical dispatcher skill for Codex. Use when Codex is asked to write, modify, refactor, or fix code and should route work through the Codex Tactical Squad architecture by deciding when to use the separate planner, executor, and reviewer skills.
---

# Codex Tactical Squad

Use this skill as the top-level dispatcher for coding tasks that should go through the Codex Tactical Squad architecture.
Its job is to tell the main thread when to use the separate tactical position skills rather than embedding all three positions in one skill.

## Position Skills

- `$codex-tactical-planner`: persistent planning and decision role
- `$codex-tactical-executor`: controlled implementation role
- `$codex-tactical-reviewer`: independent review and gatekeeping role

## Dispatcher Mission

Decide when each tactical position skill should be used.

- route task intake and boundary-setting to `$codex-tactical-planner`
- route approved implementation rounds to `$codex-tactical-executor`
- route each review gate to `$codex-tactical-reviewer`
- keep the planner as the single coordination center
- prevent the main thread from role-playing the three positions directly when the tactical squad is active

## Reuse Strategy

Do not treat all tactical positions the same.

- Reuse a persistent `$codex-tactical-planner` within the current session so boundary, cadence, and acceptance logic stay stable.
- Reuse a `$codex-tactical-executor` within the same task cluster or implementation thread, but replace it when the task domain or write scope changes substantially.
- Prefer a fresh `$codex-tactical-reviewer` for each review round so gatekeeping stays independent and less contaminated by prior implementation context.
- Prefer reusing an already-running tactical position subagent before launching a new one.

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
- [agent_registry_template.md](references/agent_registry_template.md)
- [prompt_assembly.md](references/prompt_assembly.md)

## Injection Policy

The main thread is the only runtime injection port.

- Tactical position skills define long-lived role boundaries.
- The dispatcher decides which runtime artifacts and task-local context get injected into each subagent.
- Do not expect the planner, executor, or reviewer skills to infer the current round contract on their own.
- Build each subagent prompt from the position skill plus the dispatcher-selected templates and current round state.

## Submission Contract

Each tactical position subagent must return a structured submission before the dispatcher may advance to the next DAG node.

- `$codex-tactical-planner` must submit a planning artifact.
- `$codex-tactical-executor` must submit an implementation artifact.
- `$codex-tactical-reviewer` must submit a review artifact.

Treat a tactical position node as incomplete until its required submission has been received.

Planner submission must include:

- task intake summary
- round contract
- planner decision when closing the round
- risk or residual risk notes when relevant

Executor submission must include:

- changed paths
- behavior changes
- commands run
- observed results
- commands not run
- open issues

Reviewer submission must include:

- findings
- severity and blocking status
- required fixes
- missing validation
- missing evidence

If a tactical position subagent omits required fields, the dispatcher should treat the node as not yet complete.

## Rules

- Start with a task intake and planner contract before code edits.
- If the tactical squad is active, explicitly launch the separate planner, executor, and reviewer position skills.
- Treat write scope as exact file paths plus explicitly allowed adjacent edits.
- If blocking ambiguities remain, the planner should stop and surface them instead of guessing.
- If the executor cannot finish without scope expansion or assumption changes, return a replan request instead of improvising.
- The reviewer should report findings first, ordered by severity, and use the executor report as input.
- The planner should not accept a round if the reviewer found material issues unless the residual risk is explicitly accepted.
- Do not keep all tactical positions inside the dispatcher skill itself.
- Before launching a planner or executor, first check whether a suitable tactical position subagent is already alive and reusable.
- Do not default to launching a brand-new planner and executor for every small round in the same task if reuse is practical.
- Do prefer a fresh reviewer per gate unless there is a strong reason to keep the same reviewer alive.
- Track reusable tactical position subagents in a lightweight registry or equivalent session memory.
- Inject templates and task-local runtime constraints explicitly when launching or reusing a tactical position subagent.
- Do not advance the DAG to the next node until the current tactical position submission is complete enough to satisfy its submission contract.

## Bypass

Bypass the full triad only for non-coding tasks, trivial tasks where the loop would be pure overhead, or when subagent tools or explicit subagent launch are unavailable. State the bypass reason explicitly.
