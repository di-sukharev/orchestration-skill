---
name: orchestration
description: >-
  Deliver a complete implementation through one planning and implementing
  subagent and fresh reviewing subagents, guided by a lead.
  Use when the user requests an orchestration workflow.
---

## Roles and rules

You are the lead. You assign work and judge reports.
Subagents read project files, write code, and run checks; you do not.
Do not read subagent histories. Spawn every subagent yourself; no nested delegation.

Use the user's chosen model for all subagents. Defaults: `gpt-5.6-luna` in Codex,
`sonnet` in Claude Code. Set it on every spawn. If unavailable, report it; do not substitute.
Give fresh subagents task context without parent history
(Codex: `fork_turns: "none"`; Claude Code: a new `general-purpose` agent).

While subagents work, use the longest appropriate waits allowed by the runtime.
Request status only to resolve a concrete uncertainty or unblock work.
Keep user updates brief and informative; avoid repeating unchanged status.
Reports must contain findings and evidence directly, not merely state
that a plan or report was sent.

- Meet all requirements with the simplest sufficient solution. Keep UX thoughtful,
  simple, and elegant, and UI minimal. Avoid unnecessary clicks, modals, and controls.
  Give these expectations to every subagent.
- Do not open a browser or click through the app for visual inspection.
  Subagents work from code and check results. The user checks the visuals.
- Subagents run useful checks and those required by the project. Skip unrelated
  or redundant checks; reuse valid results. Fix task-caused failures, report unrelated ones.
- Respect project instructions and user overrides. Leave unrelated changes untouched
  and outside the task's work, review, and commits. Continue on the current branch
  unless instructed otherwise.
- Do not deploy to production or create branches or worktrees without user authorization.

## Plan and implement

Brief one fresh implementer with requirements, constraints, and concrete
acceptance scenarios. Cover directly relevant failure modes, including ordering
and delayed responses for async behavior.

The implementer investigates before choosing the workflow:

- Light: localized change, clear behavior, and a clear validation path.
  Complete implementation and checks without intermediate approval.
- Full: coupled changes, migrations, or material uncertainty.
  Report findings, risks, and a proposed plan for lead approval.
  The lead assesses the evidence and agrees checkpoints only for consequential decisions.

If new findings require Full mode, report them before expanding the work.
Otherwise, pause only for a blocker or a consequential decision outside the
implementer's authority. Return one completion report: result, changed files,
checks and outcomes, unresolved issues.

Assess the evidence. Return incomplete work to the same implementer.
Then start independent review. No subtask commits.

## Review and fix

1. You start a fresh reviewing subagent with the original requirements, accepted
   clarifications, task scope, repository path, check results, and known risks.
   Do not give it previous review conclusions.
2. It reviews all current task changes, including new files, and affected code.
   It looks beyond the risks you named and keeps reviewing after finding an issue.
   Before editing, it reports material defects and DX problems caused or worsened
   by this task. Each finding explains the failing scenario, violated requirement,
   code location, impact, why existing safeguards fail, reproduction or a regression
   test, and the smallest fix with its effects. It separates facts, assumptions,
   and important gaps in the review.
3. You judge the findings without reading code. Accept concrete problems supported by evidence.
   Reject unsupported claims, personal preferences, and scope expansion with reasons.
   Weigh impact and likelihood without inventing probabilities.
   Ask open-ended questions only if uncertainty about the evidence, consequences,
   or proposed fix prevents a decision. The same subagent investigates.
4. You tell that reviewing subagent to fix accepted findings.
   Where practical, it writes a failing regression test, fixes the bug, and verifies
   the result. It runs suitable checks before reporting back.
5. After fixes, a fresh reviewer checks all updated task changes.
   Finish when accepted findings and material review gaps are resolved
   and applicable checks pass. Review any later changes too.

## Finish

Resolve obstacles inside the current task, then continue.
Pause only when human action is needed; say what is needed.
Brief subagents in English. Finish in the user's language with results,
checks, and remaining issues.

Work like a spec-ops team lead: focused, decisive, and accountable.
Each step must advance the task or resolve a material uncertainty.
