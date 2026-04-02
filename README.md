# Codex Tactical Squad

[中文说明](README.zh-CN.md) | [Installation](INSTALL.md) | [安装说明](INSTALL.zh-CN.md)

Codex Tactical Squad is a closed-loop development skill for Codex.

Its core is not "multi-agent collaboration". Its core is the separation of three powers that otherwise contaminate each other:

- decision power
- execution power
- veto power

In this skill, that separation is expressed as:

- `Planner`: define boundaries, decompose work, control cadence, make the final decision
- `Executor`: produce changes within an approved scope
- `Reviewer`: independently review and exercise gatekeeping power

This only works if three structural constraints hold:

- power separation: planning, execution, and review must not collapse into one actor
- single coordination center: all results flow back to the planner for the next decision
- gatekeeping review: the reviewer is not a suggestion source but a pass/fail gate

That is the operating philosophy:

**A closed-loop development system driven by the planner, executed by the executor, and gated by the reviewer.**

## What The Skill Does

`codex-tactical-squad` turns a coding task into a controlled loop:

1. task intake
2. planner contract
3. executor implementation
4. reviewer gate
5. planner accept, rework, or abort

The goal is to reduce two common failure modes:

- premature implementation
- self-forgiving review

## How It Works

When active, the skill requires explicit subagent launches for planner, executor, and reviewer work.
Main-thread role-play does not count as valid triad execution.

The skill uses structured artifacts for each round:

- task intake
- round contract
- implementation report
- review report
- planner decision
- replan request

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

## Bypass

Bypass the full loop only when:

- the task is not a coding task
- the task is trivial enough that the loop would be pure overhead
- subagent tools or explicit subagent launch are unavailable

When bypassing, the reason should be stated explicitly.
