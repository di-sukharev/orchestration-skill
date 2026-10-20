---
name: orchestration
description: >-
  Runs several substantial tasks in separate sub-agent contexts, with
  loop-code-review inside each task. Use when the user asks for orchestration
  or several substantial tasks.
---

Use English with sub-agents and the user's language with the user.

## Orchestrator

Start a fresh sub-agent without chat history for each task. Use a strong model (Sol or Opus) at high effort unless the user specifies otherwise.
Give each sub-agent the repository path, its task, completion criteria, relevant context, and the sub-agent instructions below.

Run tasks sequentially by default. Run them in parallel if they are independent and their edits will not conflict.
Start dependent tasks only after their prerequisites are complete.
Your role is limited to launching agents, coordinating tasks, and reporting progress, results, and blockers.
Leave all investigation, planning, implementation, checks, and review to the task agents.

## Sub-agent instructions

Read project instructions and investigate your task. Write a plan, implement it, and run the relevant checks.
Inspect the current Git state before editing and preserve existing work.
When tasks affect shared files, coordinate with the other task agents and agree on who edits each file.
Keep working on parts that do not conflict while coordination is pending.
Prefer working without a worktree; create branches or worktrees only when the user requests them.

Run [loop-code-review](https://github.com/di-sukharev/loop-code-review-skill) on your task's changes only.
Report completion only when relevant checks pass and review is `passed`.
Otherwise, report the task as incomplete and explain what remains.

Finish with the result, checks, review status, remaining issues, and a suggested commit message.
Do not commit or push unless the user explicitly asks.
