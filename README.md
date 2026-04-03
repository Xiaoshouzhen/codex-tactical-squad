# Codex Tactical Squad

[中文说明](README.zh-CN.md) | [Installation](INSTALL.md) | [安装说明](INSTALL.zh-CN.md)

Codex Tactical Squad is a closed-loop development dispatcher skill for Codex.

It is inspired in part by Anthropic's article on harness design for long-running application development:
<https://www.anthropic.com/engineering/harness-design-long-running-apps>

This is not generic "multi-agent collaboration".

It is a coding system that separates three powers that otherwise contaminate each other:

- decision power
- execution power
- veto power

In this architecture, that separation is expressed as three separate tactical position skills:

- `$codex-tactical-planner`: define boundaries, decompose work, control cadence, make the final decision
- `$codex-tactical-executor`: produce changes within an approved scope
- `$codex-tactical-reviewer`: independently review and exercise gatekeeping power

It only works if three structural constraints hold:

- power separation: planning, execution, and review must not collapse into one actor
- single coordination center: all results flow back to the planner for the next decision
- gatekeeping review: the reviewer is not a suggestion source but a pass/fail gate

**A closed-loop development system driven by the planner, executed by the executor, and gated by the reviewer.**

## What The Skill Does

`codex-tactical-squad` tells the main thread when to use the three tactical position skills and turns a coding task into a controlled loop:

1. task intake
2. planner contract
3. executor implementation
4. reviewer gate
5. planner accept, rework, or abort

The goal is to reduce two common failure modes:

- premature implementation
- self-forgiving review

## Context

If you found this project through Anthropic's harness-design article, the mapping is straightforward:

- planner -> boundary-setting and decision authority
- executor -> controlled implementation
- reviewer -> independent gatekeeping

Codex Tactical Squad turns that general harness direction into a reusable Codex dispatcher plus three position skills, with explicit subagent spawning, structured handoffs, and review-gated progress.

## How It Works

When active, the dispatcher requires explicit use of the separate planner, executor, and reviewer position skills.
Main-thread role-play does not count as valid tactical-squad execution.
The dispatcher is also the only runtime injection port: it decides which templates and task-local constraints get injected into each tactical position subagent.

It does not assume that all three roles should be recreated every time.

- `$codex-tactical-planner`: prefer session-level reuse
- `$codex-tactical-executor`: prefer reuse within the same task cluster
- `$codex-tactical-reviewer`: prefer a fresh reviewer per gate

This keeps boundary and execution continuity while preserving review independence.
In practice, the dispatcher should check for a live planner or executor first and only spawn a new one when reuse is no longer suitable.

The skill uses structured artifacts for each round:

- task intake
- round contract
- implementation report
- review report
- planner decision
- replan request

Each tactical position must submit its required structured artifact before the dispatcher may advance to the next step.

## Position Skills

- `$codex-tactical-planner`
- `$codex-tactical-executor`
- `$codex-tactical-reviewer`

## Install

See [INSTALL.md](INSTALL.md).

## Usage

Invoke the skill explicitly:

```text
Use $codex-tactical-squad to handle this coding task:
Goal:
Constraints:
Definition of done:
Relevant files:
```

Example:

```text
Use $codex-tactical-squad to handle this coding task:
Goal: add pause and resume controls to the training workflow
Constraints: do not refactor unrelated UI modules
Definition of done: pause and resume work correctly and existing tests do not regress
Relevant files: training/runtime.py, views/workbench_window.py
```

## Included References

- `references/task_intake_template.md`
- `references/round_contract_template.md`
- `references/implementation_report_template.md`
- `references/review_report_template.md`
- `references/planner_decision_template.md`
- `references/replan_request_template.md`
- `references/orchestrator_checklist.md`
- `references/spawn_protocol.md`
- `references/agent_registry_template.md`
- `references/prompt_assembly.md`
- `references/submission_contract.md`

## Bypass

Bypass the full loop only when:

- the task is not a coding task
- the task is trivial enough that the loop would be pure overhead
- subagent tools or explicit subagent launch are unavailable

When bypassing, the reason should be stated explicitly.
