---
name: orchestration
description: >-
  Runs several substantial tasks in separate sub-agent contexts, with
  loop-code-review inside each task. Use when the user asks for orchestration
  or several substantial tasks.
---

Keep task contexts separate. Each sub-agent owns one task from planning through review.
Follow project rules. Use English with sub-agents and the user's language with the user.
Requires [loop-code-review](https://github.com/di-sukharev/loop-code-review-skill).

## Orchestrator

Start a fresh sub-agent without chat history for each task. Use a strong model (Sol or Opus) at high effort unless the user specifies otherwise.
Give each sub-agent one task from the user's list and the sub-agent instructions below.

Run tasks sequentially unless they are clearly independent and can run without conflicting edits.
Start dependent tasks only after their prerequisites are complete.
Keep your context small. You manage the tasks; the sub-agents do the work and check it through `loop-code-review`.
Report task progress, results, and blockers.

## Sub-agent instructions

Read project instructions and investigate your task. Write a plan, implement it, and run the relevant checks.
Inspect the current Git state before editing and preserve existing work.
Communicate directly with other active sub-agents or sessions when possible. Agree on who changes what to avoid conflicts.
Keep working on parts that do not conflict while coordination is pending.
Prefer working without a worktree; create branches or worktrees only when the user requests them.

Run `loop-code-review` on your task's changes, preserving other work. Let it manage the reviewer sub-agents and review loop.
Report completion only when review is `passed`. If the required review cannot run, report the task as incomplete.

Finish with the result, checks, review status, remaining issues, and a suggested commit message.
Do not commit or push unless the user explicitly asks.
