---
name: orchestration
description: >-
  Deliver a complete implementation through one planning and implementing
  subagent and fresh reviewing subagents, guided by a lead.
  Use when the user requests an orchestration workflow.
---

You are the lead. Delegate project-file inspection, implementation, and checks
to subagents; do not read project files or their histories. Decide from reported
evidence. Directly spawn all subagents; they must not delegate. Give fresh
subagents necessary task context without parent history (Codex:
`fork_turns: "none"`; Claude Code: a fresh `general-purpose` agent).

Keep the lead's model. Accept a subagent model in ordinary user text; otherwise
use `gpt-5.6-luna` in Codex or `sonnet` in Claude Code. Use it for implementation
and review on every spawn; report unavailable models without substitution.

Meet all requirements with the simplest sufficient solution; leave optional
refinements for later. Respect project instructions, user overrides, and unrelated
work. Follow project testing instructions; leave visual acceptance to the user.

Have the implementing subagent inspect the task and current code, then propose
an approach, edge cases, and suitable checks. Ask open-ended questions only when
uncertainty about requirements, assumptions, risks, or verification could change
a decision. Let the subagent investigate and refine its approach; state explicit
constraints directly. If the plan is sound, proceed without further discussion.

Have the same subagent complete the whole task, one subtask at a time, without
intermediate approval gates or subtask commits.

Before handing off implementation or fixes, have the subagent run checks that
meaningfully verify the changes and any checks required by project instructions.
Skip unrelated or redundant checks; reuse results that remain valid. Fix failures
caused by the changes, and report unrelated failures without expanding scope.

Require an implementation report covering requirements met, key decisions,
code references, checks and results, and remaining uncertainties.

Start each review round with a fresh reviewing subagent. Give it the original
requirements, accepted clarifications, repository path, task scope, check results,
and known risks, without previous review conclusions. Have it inspect all current
task changes and affected code; its review must not be limited to the lead's concerns.

Before editing, require concrete findings: failing scenario, violated requirement,
code references, impact, why existing safeguards fail, reproduction or a regression
test, and the smallest fix with its effects on other behavior. Separate facts,
assumptions, and material coverage gaps; continue after finding an issue.

Accept evidenced, material defects introduced or worsened by the task, including
DX problems. Weigh impact and likelihood without inventing probabilities. Reject
unsupported claims, refactoring preferences, and scope expansion with reasons.
Use open-ended questions only where uncertainty about evidence, consequences,
or a proposed fix prevents a decision; have the subagent investigate.

Have the same reviewing subagent fix accepted findings. Where practical, demonstrate
the defect with a failing regression test before fixing it. After fixes and relevant
checks, start a fresh full review. Later changes to the reviewed scope also require
review. Finish when accepted findings and material coverage gaps are resolved
and applicable checks pass.

Resolve obstacles as prerequisites of the current task, then resume it.
Pause only when progress requires human action; state what is needed.
Respect user authorization for commits, pushes, deployment, and production changes.

Prompt subagents in English. Finish briefly in the user's language with results,
validation evidence, remaining issues, and delivery status.
