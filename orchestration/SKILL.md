---
name: orchestration
description: Deliver a complete implementation efficiently through a lead, a reused Luna worker, and fresh reviewers. Use when the user requests this orchestration workflow.
---

You are the lead. Deliver a fully sufficient implementation with the least total
elapsed time and token cost, including rework. Own the result; let subagents write
the code. Adapt the process to the task instead of optimizing for ceremony or
speculative improvements.

Keep your current model; use `gpt-5.6-luna` for subagents unless the user chooses
otherwise. Set the model through the spawning tool. If unavailable, report it
without silently substituting. Follow project instructions, preserve unrelated
work, and stay within the user's authorization. A plan-only request ends at the plan.

Delegate initial code discovery to Luna. Ask for a short map of relevant files,
behavior, checks, and gaps, then inspect the needed code yourself and make a plan.
Do not read subagent histories or transcripts. Exchange concise briefs and reports,
using JSON or free-form text, whichever is clearer and cheaper.

Prefer small sequential subtasks for the same Luna worker so it retains useful
knowledge. Give each a precise Definition of Done: the desired behavior, a high-level
implementation idea, important pitfalls, and how to verify completion. Explain only
what helps it succeed; leave the actual code to the worker. Check its short report
and relevant code as needed before moving on. Adjust task size or start a fresh
worker when that would improve the result or remove bias. Give fresh agents enough
requirements, paths, and constraints to work independently, without chat history.

After implementation, have a fresh Luna reviewer examine all active task changes,
including new files and interactions. Tailor the brief to the real risks. It first
returns a short, substantiated list of problems without editing. Decide what needs
verification and which findings you accept, then direct that same reviewer to make
the fixes and check them. After fixes, use another fresh reviewer without prior
conversation or review conclusions. Continue until an independent pass finds no
remaining actionable issues and the implementation meets the requirements with
appropriate checks passing. If progress stalls, address the cause or report the
blocker rather than repeating an unproductive loop or claiming success.

Avoid redundant research, checks, handoffs, and reports. Save time and tokens by
removing unnecessary work, not by skipping requirements or evidence of correctness.
Finish with a brief account of the result, checks, and remaining limitations.
