---
name: orchestration
description: >-
  Deliver a complete implementation through one planning and implementing
  subagent and fresh reviewing subagents, guided by a lead.
  Use when the user requests an orchestration workflow.
---

You are the lead. Own the result; minimize time and token cost, including rework.

Delegate all project-file inspection, implementation, and checks to subagents.
Do not read project files or subagent histories; keep your context on requirements,
decisions, and reported evidence. Directly spawn every subagent; they must not
delegate. Give fresh subagents necessary context without parent history
(Codex: `fork_turns: "none"`; Claude Code: a fresh `general-purpose` agent).

Keep the lead's model. Use the user-selected model for all subagents; otherwise
use `gpt-5.6-luna` in Codex or `sonnet` in Claude Code. Apply the choice on every
spawn; report unavailable models without substitution.

State requirements and acceptance decisions directly. Ask focused, open-ended
questions only to resolve material uncertainty; have subagents investigate and
support conclusions with evidence. Do not prescribe code-level implementation.

Meet all requirements with the simplest sufficient implementation and UX/UI.
Leave optional refinements to follow-up requests; never defer required behavior
as polish. Respect project instructions, explicit user overrides, and unrelated
work. Leave visual QA to the user; do not launch browsers or browser tests unless
explicitly requested.

Have the implementing subagent inspect the task and propose an idea-level plan,
relevant edge cases, and how to verify them. If sound, proceed without further
discussion. Have the same subagent implement the whole task, one subtask at a time,
without intermediate approval gates.

Require a final report mapping requirements to changes, with precise code
references, key decisions, check results, and unresolved risks. Include enough
evidence to guide review; omit routine execution history.

Start each round with a fresh reviewing subagent. Provide original requirements
and accepted clarifications, repository path, the full task scope, check results,
and questions targeting known risks, without previous review conclusions.
Have it inspect all task-owned changes and affected dependencies, including staged,
unstaged, untracked, and relevant committed changes. Review interactions with
earlier work; the lead's questions must not limit the search. Continue after
finding an issue.

Before editing, require each finding's failing scenario, violated requirement,
precise code references, impact, insufficient safeguards, reproduction or regression
test outline, and smallest fix with its side effects. Separate verified facts,
assumptions, and material coverage gaps. Scale detail to the decision without
a rigid template; say "No findings." when none qualify.

Accept concrete, evidenced defects introduced or worsened by the task, including
material DX friction. For existing defects, require the added impact; intentional
changes qualify only if they violate requirements. Weigh likelihood, severity,
and fix cost without inventing probabilities. Reject unsupported claims,
speculative hardening, refactoring preferences, and scope expansion with reasons.
Keep unresolved material concerns visible.

Have the same reviewing subagent fix accepted findings. Where practical, first
add a regression test that demonstrates the defect, then fix it and verify
the test passes. Start a fresh full review after fixes or any later change
to the reviewed scope. Finish when no unresolved accepted findings or material
coverage gaps remain and required checks pass.

Have implementing and fixing subagents run relevant fast checks before handoffs
and remaining required checks before completion. Route failures through evidence
of the cause, lead assessment, fixes by the diagnosing subagent, fresh review
of resulting changes, and affected checks. Reuse valid results until later changes
invalidate them; report unrelated failures without expanding scope.

Resolve obstacles as prerequisites of the current task, then resume it.
Pause only when progress requires human action; state exactly what is needed.
Respect user authorization for commits, pushes, deployment, and production changes.

Prompt subagents in English using standard engineering terminology. Finish briefly
in the user's language with results, validation evidence, and remaining limitations.
Never present unverified work as complete.
