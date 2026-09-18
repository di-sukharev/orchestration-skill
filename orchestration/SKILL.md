---
name: orchestration
description: >-
  Coordinate a complete implementation through subagents.
  Use one subagent to plan and implement. Use new subagents for independent review.
  Use when the user requests an orchestration workflow.
---

This workflow requires the installed `loop-code-review` skill.
Before you start review, read its `SKILL.md`.

## Roles and rules

You are the lead. Assign work. Evaluate reports.
Subagents read project files, write code, and run checks.
Do not do these tasks yourself.
Start each subagent yourself.
Do not let subagents start other subagents.
Do not read subagent histories.

Use the user's selected model for all subagents.
If the user does not select a model, use `gpt-5.6-luna` in Codex or `sonnet` in Claude Code.
Set the model each time you start a subagent.
If the selected model is not available, tell the user.
Do not use another model.

Give each new subagent the task context without the parent conversation history.
In Codex, set `fork_turns: "none"`.
In Claude Code, start a new `general-purpose` agent.

While subagents work, use the longest wait that is appropriate and allowed by the runtime.
Request status only to resolve a specific uncertainty or allow blocked work to continue.
Keep user updates short and informative.
Do not repeat unchanged status.
Require findings and evidence in the report itself.
Do not accept a report that only says the subagent sent a plan or report.

- Meet all requirements with the simplest sufficient solution.
  Keep the UX thoughtful, simple, and elegant.
  Keep the UI minimal.
  Avoid unnecessary clicks, modals, and controls.
  Give these requirements to every subagent.
- Do not open a browser for visual inspection.
  Do not click through the application for visual inspection.
  Subagents use code and results from checks to evaluate the work.
  The user checks the visuals.
- Subagents must run useful checks and all checks that the project requires.
  Skip unrelated or redundant checks.
  Reuse results that are still valid.
  Fix failures caused by the task.
  Report unrelated failures.
- Follow project instructions and user overrides.
  Do not modify unrelated changes.
  Exclude unrelated changes from the task's work, review, and commits.
  Unless instructed otherwise, continue on the current branch.
- Without user authorization, do not deploy to production, create branches, or create worktrees.

## Plan and implement

Start one new implementer.
Give the implementer the requirements, constraints, and concrete acceptance scenarios.
Include failure modes that are directly relevant to the task.
For asynchronous behavior, include ordering and delayed responses.

Tell the implementer to investigate before choosing a mode:

- **Light:** Use this mode for a localized change with clear behavior and a clear validation path.
  Complete implementation and checks without intermediate approval.
- **Full:** Use this mode for changes that depend on each other, migrations, or material uncertainty.
  Report findings, risks, and a proposed plan to the lead for approval.

As the lead, assess the evidence for the Full plan.
Agree on checkpoints only for consequential decisions.

If new findings require Full mode, the implementer must report the findings before expanding the work.
Otherwise, the implementer must pause only for a blocker or a consequential decision outside the implementer's authority.
Require one completion report with the result, changed files, checks and their outcomes, and unresolved issues.

Assess the completion report's evidence.
If the work is not complete, ask the same implementer to complete it.
After implementation is complete, start independent review.
Do not commit individual subtasks.

## Review and fix

Run `loop-code-review` for all current task changes.
You remain the lead.
Use the selected subagent model.
Do not start a separate coordinator.
Supply the original requirements, accepted clarifications, task scope, repository path, implementation report, check results, and known risks.
Before you finish the task, complete the review and fix process from `loop-code-review`.

## Finish

Resolve obstacles within the current task.
Then continue the work.
Pause the task only when human action is necessary.
If human action is necessary, tell the user what action is needed.
Write subagent instructions in English.
Write the final response in the user's language.
Include results, checks, and remaining issues.

Use each step to advance the task or resolve a material uncertainty.
