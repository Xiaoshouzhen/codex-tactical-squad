# Prompt Assembly

Use this file to decide which templates and runtime context the dispatcher should inject into each tactical position subagent.

## Planner Injection

Inject:

- the current task intake
- the current tactical position registry summary when relevant
- `task_intake_template.md`
- `round_contract_template.md`
- `planner_decision_template.md`
- the latest reviewer report when making an accept, rework, or abort decision

Planner runtime prompt should answer:

- what is this round
- what is in scope
- what is out of scope
- what are the acceptance criteria
- what is the next decision

## Executor Injection

Inject:

- the approved round contract
- explicit write scope
- relevant repo slices or changed paths
- `implementation_report_template.md`
- `replan_request_template.md`

Executor runtime prompt should answer:

- what exactly to change
- which files are allowed
- what validation to run
- how to report results
- when to stop and request replan

## Reviewer Injection

Inject:

- the original task or task summary
- the approved round contract
- the executor implementation report
- relevant changed paths or diffs when available
- `review_report_template.md`

Reviewer runtime prompt should answer:

- what to check
- what evidence is missing
- whether the round should be blocked
- what fixes are required before acceptance
