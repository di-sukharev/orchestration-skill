---
name: orchestration
description: >-
  Coordinate a complete implementation through subagents.
  Use one subagent to plan and implement. Use new subagents for independent review.
  Use when the user requests an orchestration workflow.
---

## Roles and rules

You are the lead. Assign work. Assess reports.
Delegate project file inspection, code changes, and checks to subagents.
Do not do these tasks yourself.
Start every subagent yourself.
Do not let subagents start other subagents.
Do not read subagent histories.

Give each new subagent the task context without the parent history.
In Codex, set `fork_turns: "none"`.
In Claude Code, start a new `general-purpose` agent.

Use the longest wait appropriate for the work and permitted by the runtime.
Request status only to resolve uncertainty or unblock work.
Send brief, informative updates when status changes.
Require reports to contain findings and evidence.

Give every subagent these rules:

- Meet all requirements with the simplest sufficient solution.
  Keep the UX thoughtful, simple, and elegant.
  Keep the UI minimal.
  Avoid unnecessary clicks, modals, and controls.
- Do not use a browser for visual inspection.
  Assess the code and check results.
  The user checks the visuals.
- Run useful checks and all checks that the project requires.
  Skip unrelated or redundant checks.
  Reuse valid results.
  Fix failures caused by the task.
  Report unrelated failures.
- Follow project instructions and user overrides.
  Do not modify unrelated changes.
  Exclude unrelated changes from the task's work, review, and commits.
  Unless instructed otherwise, continue on the current branch.
- Without user authorization, do not deploy to production, create branches, or create worktrees.

## Model and effort selection

Choose model and effort separately for each implementation, review, and fix assignment.
Honor explicit user choices and budget limits within their stated scope.
Choose unspecified settings without routine approval.
Use the current runtime's supported options and capability descriptions.
Do not assume a fixed model catalog or infer capabilities from names.

Never select effort below `medium`, including inherited and fallback settings.
Use `high` by default.
Choose the least costly reliable option, including time and retries in the cost:

| Task | Model | Effort |
| --- | --- | --- |
| Clear, localized, low-risk work with obvious checks | Lightweight | `medium` |
| Related changes, ordinary debugging, or some uncertainty | Balanced | `high` |
| Architecture, migrations, concurrency, security, or unclear failures across components | Stronger reasoning | `xhigh` when justified and supported; otherwise `high` |

Treat these as starting points, not fixed pairs.
Assess uncertainty, dependencies, and error consequences, not just file count or role.
Choose reviewers for the risks they must detect, independently of the implementer's settings.

If an automatic selection is unavailable, choose another suitable option.
If no alternatives are exposed, use inherited or default settings that meet the effort minimum.
If a user choice or the effort minimum is unavailable, report the limitation.
Do not silently replace an explicit user choice.

Set model and effort explicitly through supported tool parameters:

- Codex: `model` and `reasoning_effort`.
- Claude Code: the model selector, plus effort if exposed.

Do not simulate unsupported effort with prompt wording.
Briefly explain the settings when you start an agent or change them.

If complexity increases or repeated attempts stall, reassess the settings within the user's limits.
Increase effort for deeper reasoning or select a stronger model for broader capabilities.
Resolve missing information, permissions, and tool failures directly.
Before you replace an agent, stop it.
Give its replacement the scope, findings, changes, and check results.
Keep independent review rounds fresh, without prior review conclusions.

## Plan and implement

Start one new implementer.
Supply requirements, constraints, concrete acceptance scenarios, and relevant failure modes.
For asynchronous behavior, include ordering and delayed responses.

Tell the implementer to investigate before choosing a mode:

- **Light:** A localized change with clear behavior and checks.
  Complete implementation and checks without intermediate approval.
- **Full:** Dependent changes, migrations, or material uncertainty.
  Report findings, risks, and a proposed plan to the lead for approval.

Assess the evidence for the Full plan.
Agree on checkpoints only for consequential decisions.

If new findings require Full mode, require a report before the implementer expands the work.
Otherwise, permit pauses only for blockers or consequential decisions outside the implementer's authority.
Require one completion report: result, changed files, check outcomes, and unresolved issues.

Assess the completion report's evidence.
Return incomplete work to the same implementer unless the selection rules require a stronger model or effort.
After implementation is complete, start independent review.
Do not commit individual subtasks.

## Review and fix

Before review, read the installed `loop-code-review` skill's `SKILL.md`.
Run its complete review and fix process for all current task changes.
Remain the lead.
Do not start a separate coordinator.
Apply this model and effort policy instead of that skill's default model selection.
Supply the original requirements, accepted clarifications, task scope, repository path, implementation report, check results, and known risks.

## Finish

Resolve obstacles within the task scope.
Continue until completion.
Pause only when human action is necessary.
State the required action.
Write subagent instructions in English.
Write the final response in the user's language.
Include results, checks, and remaining issues.
