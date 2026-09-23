---
name: orchestration
description: >
  Coordinate research, planning, sequential implementation, and independent
  review through subagents. Use when the user requests orchestration.
---

## Rules

Minimize total token cost, including retries.
Own decisions and the plan.
Delegate code inspection, edits, and checks.
Only you can start agents.
Do not read agent histories.

Follow project rules and user choices.
Preserve unrelated changes.
Use supported models and effort settings.

Use Luna in Codex or Sonnet in Claude Code for research and implementation.
Choose review models and effort by risk.
Use stronger models when complexity requires them.

Start new agents without parent history.
In Codex, use `fork_turns: "none"`.

Give each agent the repository, scope, constraints, relevant evidence,
expected result, and checks.
Request results, file references, check outcomes, and blockers.
Reuse valid evidence. Request updates only when needed for a decision.

## Research

Check that `loop-code-review` is available.
Start one agent for read-only research.
Request project rules, initial working-tree state, relevant code paths,
reusable APIs, affected callers, checks, and consequential unknowns.

## Plan

Write the plan from the research evidence.
Use ASD-STE100 clarity principles: short sentences and consistent terms.

Define:
- Scope and observable Definition of Done.
- The simplest complete UX, including required states.
- Code ownership and reuse that keep implementation and maintenance simple.
- Ordered subtasks, dependencies, expected results, and checks.
- Relevant failure cases and required safeguards.

Where needed, specify transaction boundaries, rollback, asynchronous ordering,
retries, duplicate handling, permissions, compatibility, and migrations.
Resolve consequential unknowns before dependent work.

## Implement

Assign one subtask at a time. Reuse the research agent when useful.
Accept each result against the plan.
Return incomplete work to its agent.
Revise the plan when evidence changes.
Stop an agent before replacing it.

## Review

After implementation, run `loop-code-review` on all task changes.
Remain the coordinator. Apply this skill's model policy.
Give fresh reviewers the requirements, scope, checks, and known risks.
Exclude previous review conclusions.

## Finish

Confirm that the Definition of Done is met.
After review passes, commit and push only task changes within user authorization.
Report the result, checks, remaining issues, commit, and push status.
