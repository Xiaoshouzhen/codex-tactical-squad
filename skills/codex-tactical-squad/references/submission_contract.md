# Submission Contract

Use this file to decide whether a tactical position node is complete enough for the dispatcher to advance.

## Planner Submission

Required fields:

- task intake summary
- round contract
- planner decision when closing the round
- risk or residual risk notes when relevant

## Executor Submission

Required fields:

- changed paths
- behavior changes
- commands run
- observed results
- commands not run
- open issues

## Reviewer Submission

Required fields:

- findings
- severity and blocking status
- required fixes
- missing validation
- missing evidence

## Dispatcher Rule

Do not advance to the next DAG node until the current tactical position submission is complete enough to satisfy its required fields.
Treat status updates as non-terminal unless the dispatcher explicitly closes the node or the node is genuinely blocked.
