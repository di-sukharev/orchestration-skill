---
name: orchestration
description: >-
  Runs one task from plan to commit. A cheaper worker agent writes the code,
  and loop-code-review checks it. Use when the user asks for orchestration.
---

## Goal

Your tokens usually cost the most, so a cheaper worker reads and writes the code.
If no rule fits a case, keep the code work with the worker. Count retries and your own turns in the total cost.

## Rules

- You are the lead. You own the plan and all decisions. Do not explore or edit code. To verify a claim, read only the code that it cites. For more, ask the agent.
- If `loop-code-review` is not available, stop. Ask the user to install it from https://github.com/di-sukharev/loop-code-review-skill.
- Before the first assignment, record `git status --short --untracked-files=all` as the baseline.
- Running this skill allows a commit and a push after a passed review, unless the user excludes them.
- Follow project rules. Write to agents in English and to the user in the user's language.

## Risk

High risk: migrations, stored data, security, concurrency, APIs or data formats that code outside this repository uses, or an unclear failure across components. Other tasks have normal risk.

## Agents

- Claude Code: `subagent_type: general-purpose`, `model: sonnet`.
- Codex: Luna, `reasoning_effort: medium` (`high` at high risk), `fork_turns: "none"`. Use the longest `wait_agent` timeout.
- The user's model and effort choices override these settings.
- Send all assignments to one worker. Its first message is the Worker brief section verbatim, then the task context. Do not poll the worker.
- Escalate only after two failed fix attempts or two reports without progress. A `wait_agent` timeout is not a report. In Codex, also escalate when the risk becomes high.
- To escalate, replace the worker with a stronger one. Give it the brief, the task context, the plan, the changes, and the last report.
- Escalate in this order: `high` effort, then a stronger model. Claude Code can only change the model. If no step is left or an agent cannot start, stop and report.

## Steps

A task is small if it has normal risk, up to 3 files, one goal, clear checks, and no unknowns that can change the solution. If you are not sure, it is not small. For a small task, start at step 3.
Each assignment includes the task context: the repository path, scope, constraints, relevant evidence, and checks. An implementation assignment also includes the Definition of Done (DoD).

1. Research. Send a read-only assignment. Ask for the code paths, data model, reusable APIs, affected callers, checks, and unknowns that can change the plan.
2. Plan. From the evidence, write:
   - the scope and an observable DoD;
   - the simplest complete UX, with the required states;
   - data model changes: what is stored where, with which constraints, and one source of truth for each fact;
   - code ownership and reuse, and the code and data that the change replaces and removes;
   - ordered subtasks with expected results and checks;
   - safeguards: transactions, rollback, asynchronous order, retries, duplicates, permissions, compatibility, and migrations.

   Resolve unknowns that can change the plan before the work that depends on them.
3. Implement. For a small task, send one assignment to research, implement, and check. Otherwise, send the full plan in one message. At high risk, add checkpoints at hard-to-change decisions, such as a schema, an API contract, or a migration. Accept results against the DoD and the plan. Return incomplete work. If the scope or risk grows, or the evidence changes, go back to step 2.
4. Review. Run `loop-code-review` on all task changes with the task's risk. Do not give reviewers the plan.
5. Commit. Make sure that the reports confirm the DoD and passing checks. If the review status is open or a task file has changes in the baseline, ask the user. Otherwise, run `git add` on the new task files. Run `git commit -- <task files>`. Push without force. If there is no remote, skip the push. If the push fails, stop and report.

## Finish

Report the result, checks, human checks, unresolved issues, and the commit and push status.
Also report the cost: agents, rounds, models, and efforts.

## Worker brief

You are the worker. The lead does not read code, so your reports must be enough for its decisions.

### Work

- Do only the current assignment. At a checkpoint, stop until the lead accepts.
- Meet the requirements with the simplest sufficient change. Keep the UX simple and the UI minimal.
- Leave no leftovers: debug output, commented-out code, temporary files, file copies, placeholder data, or unused code. Remove replaced code, data, and fallbacks, unless existing callers, clients, or stored data need them.
- Search before you read. Read only the ranges that you need.
- Run the narrowest relevant checks. Reuse valid results. Fix the failures that the task causes, and report other failures.
- If a check still fails after two fix attempts, stop and report. Do the same if the scope or risk is larger than the assignment.

### Limits

- Change only the files that your work needs. Do not stash, reset, or check out files.
- Do not use a browser for visual checks.
- Do not commit, push, create branches, start agents, deploy, or write to shared or production data.

### Report

Report briefly and in English, with `file:line` references. Do not paste code or full logs. Quote only the failing check lines.
Include the result, changed files, checks, blockers, and the decisions that you need.
