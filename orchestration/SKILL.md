---
name: orchestration
description: >
  Coordinate research, planning, implementation, and independent review
  through subagents at the lowest total cost. Use when the user requests orchestration.
---

## Rules

- You are the lead. You own the route, the plan, and all decisions.
- Minimize the total cost of each completed task. Include retries and your own turns.
- Agents inspect code, edit code, and run checks. To settle a decision, you can read up to 100 lines of code. Do not explore or edit.
- Only you start agents. Start each agent without parent history. Do not read agent histories.
- Do not poll agents. Wait for their reports.
- Write agent messages in English. Write the final report in the user's language.
- Follow project rules and user choices. User model and effort choices override this skill.
- Before the first assignment, check that `loop-code-review` is available.
- Before the first assignment, record `git status --short --untracked-files=all`. Use it to separate the task files from earlier changes.
- Preserve unrelated changes.

## Route

Choose a route before the first assignment. If you are not sure, choose M.
If the worker reports a larger scope or a higher risk, change the route.

| Route | When | Flow |
| --- | --- | --- |
| S | One change chain, up to 3 files, clear checks, low risk | One assignment: research, implement, check |
| M | Related changes or material uncertainty | Research, plan, implement |
| L | High risk: migrations, data, security, concurrency, public contracts, or failures across components | Research, plan, implement with checkpoints |

## Worker

One worker researches, implements, and fixes its own failures.
Send each new assignment to the same worker with `SendMessage` (Claude Code) or `send_input` (Codex).

- Claude Code: `subagent_type: effort-medium` and `model: sonnet`. For route L, use `effort-high`. If no agent type matches the chosen effort, use the nearest type and tell the user. If these agent types are missing, use `general-purpose` and tell the user that the worker inherits the session effort.
- Codex: Luna, `reasoning_effort: medium` (`high` for route L), and `fork_turns: "none"`. Use the longest `wait` timeout.
- Escalate one step only when the route changes to L, checks fail after two fix attempts, or the worker stalls. A stall is two reports without progress. A `wait` timeout is not a stall. The steps are `medium`, `high`, and a stronger model. Stop the old worker. Give the new worker the plan, findings, changes, and check results.

## Brief

Send this brief in the first message to the worker:

- Meet the requirements with the simplest sufficient change. Keep the UX simple and the UI minimal.
- Search before you read. Read only the ranges you need.
- Run the narrowest relevant checks. Reuse valid results. Fix failures that the task causes. Report other failures.
- If a check still fails after two fix attempts, stop and report.
- Do not use a browser for visual checks.
- Do not commit, push, create branches, or start agents.
- If the scope or risk exceeds the assignment, stop and report.
- Report briefly and in English: result, changed files, checks, blockers, and decisions needed. Use `file:line` references. Do not paste code or full logs. Quote only failing check lines.

Add the repository path, scope, constraints, relevant evidence, and checks.
Add the Definition of Done (DoD) to each implementation assignment.

## Plan

For routes M and L, the first assignment is read-only research.
Request the relevant code paths, reusable APIs, affected callers, checks, and consequential unknowns.

Write the plan from the evidence:

- Scope and an observable DoD.
- The simplest complete UX, including the required states.
- Code ownership and reuse.
- Ordered subtasks with expected results and checks.
- Relevant failure cases and safeguards: transactions, rollback, asynchronous order, retries, duplicates, permissions, compatibility, and migrations.

Resolve consequential unknowns before dependent work.

## Implement

- Send the full plan to the worker in one message.
- For route L, add checkpoints at consequential decisions, such as a schema, an API contract, or a migration. The worker stops at each checkpoint for acceptance.
- Accept results against the plan. Return incomplete work to the worker.
- When the evidence changes, revise the plan.

## Review

Run `loop-code-review` on all task changes.
Give it the risk (high for route L, normal for routes S and M), the requirements with accepted clarifications, the DoD, the task files, the check results, and the known risks.
Do not give it the plan or the worker's conclusions.

## Finish

- Confirm the DoD and the passing checks from the reports.
- If the review status is open, report it and ask the user.
- If the review status is passed, commit and push only the task changes, within user authorization. Add new task files, then run `git commit -- <task files>`.
- Do not commit a file that had changes before the task. Report it.
- Report the result, checks, remaining issues, and commit and push status.
- Report the cost: route, agents, rounds, models, efforts, and subagent tokens if the runtime reports them.
