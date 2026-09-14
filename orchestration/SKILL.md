---
name: orchestration
description: Plan and implement a feature through a lead agent, compact Luna code discovery, sequential Luna workers, and fresh Luna reviewers who fix their own findings. Use when the user requests this delegated implementation workflow or invokes orchestration.
---

# Orchestration

You are the lead. Own the plan, integration, validation evidence, and final result.
Delegate initial discovery, implementation, and review fixes to Luna subagents.
Read relevant code yourself after discovery; do not write implementation code.
This skill is self-contained and does not require other skills.

## Scope and models

- Keep the lead on the current conversation model. Default every scout, worker,
  and reviewer to `gpt-5.6-luna`. Honor an explicit user model override.
- Apply the model through the spawning tool, not merely in the agent's prompt.
  Start agents without parent history (`fork_turns: "none"` or equivalent).
- Confirm the host supports delegation and the selected model before starting.
  If unavailable, report `BLOCKED` and the missing capability; do not silently
  substitute a model or implement the task yourself.
- Give each agent the repository path, task scope, acceptance criteria, applicable
  user constraints, and paths to project instructions. Never assume a fresh agent
  inherited these. Keep implementation and review sequential to avoid concurrent
  writes to the shared checkout.
- Respect plan-only requests and active planning modes: perform discovery and
  produce the plan, then stop. For an authorized implementation request, show the
  plan and continue without adding a new approval gate.
- Inspect initial Git status and record pre-existing changes, including untracked
  files. Preserve unrelated work. The review scope is all active task-owned changes
  together, including new files; do not claim unrelated changes were reviewed.
  Resolve genuinely ambiguous ownership before modifying overlapping work.
- Follow project instructions and existing authorization. The skill does not grant
  permission to commit, push, deploy, mutate remote data, or add paid dependencies.

## 1. Narrow discovery

Before searching implementation code yourself, send one Luna scout to map the
requested feature. You may first read project instructions and product docs and
inspect Git status. Ask the scout to inspect code read-only and return only compact
JSON: relevant paths, symbols, responsibilities, existing checks, and uncertainties.
Use this shape, omitting irrelevant entries rather than expanding the schema:

```json
{
  "areas": [
    {"path": "src/example.ts", "symbols": ["example"], "role": "Owning behavior"}
  ],
  "flow": ["entry point -> business rule -> persistence -> response"],
  "checks": [{"path": "tests/example.test.ts", "command": "project test command"}],
  "risks": ["Directly coupled behavior to inspect"],
  "unknowns": ["What was not found or verified"]
}
```

Request only the most relevant files, no source dumps or exhaustive inventory.
The scout should verify paths and commands, distinguish evidence from guesses,
and expose gaps rather than implying complete coverage.

## 2. Inspect and plan

Read the code identified by the JSON and the directly coupled callers/consumers.
Treat the JSON as a search map, not proof. Expand the search only where a concrete
gap requires it; delegate another bounded search if the missing area is large.

State the intended product behavior and write a concise implementation plan:
subtasks in dependency order, owning areas, acceptance criteria, and focused
validation. Resolve material product ambiguity with the user; make routine
engineering decisions yourself. Group small coupled changes when splitting them
would add handoffs without improving correctness.

## 3. Implement through workers

For each subtask or coherent small group:

1. Start a fresh Luna worker with the plan slice, useful paths, relevant decisions,
   acceptance criteria, validation requirements, and ownership boundaries.
2. Ask it to inspect the actual code, implement the slice, and run appropriate
   project checks. For a reproducible behavior bug, capture a failing regression
   test before fixing it when the existing infrastructure supports that.
3. Require a compact report: changed files, behavior delivered, checks and outcomes,
   remaining issues, and any deviation from the plan. Avoid full logs unless needed
   to diagnose a failure. Do not mark unrun checks as passing.
4. Inspect the result and the relevant diff yourself. If acceptance criteria are
   incomplete, return the worker to the missing work before starting the next slice.

The lead can run checks and inspect code but delegates every implementation edit.
Keep the queue and integration decisions in the lead's context, not worker histories.
After all slices, verify the combined result against the plan and run any directly
coupled integration checks not already covered by passing checks of this state.

## 4. Fresh review and fix loop

Start a new Luna reviewer after implementation. It must review the entire active
task diff, including untracked/new files and interactions between subtasks.
Give it the original requirements, current scope and baseline, project instructions,
and validation commands/results. Do not pass implementation chat history, earlier
review reports, earlier verdicts, or the author's rationale as a conclusion to trust.

Tailor the review brief to actual risks. For example, ask about concurrency and
transaction boundaries for persistence; authorization and input validation at trust
boundaries; retries, cancellation, idempotency, and error visibility for async work;
producer/consumer compatibility for contracts. Do not invent irrelevant audit work.

The reviewer must:

1. Independently inspect the combined changes and relevant surrounding behavior.
2. Identify substantiated bugs, missed requirements, regressions, and material
   maintainability problems. Explain a concrete failure or impact, not speculative
   style preferences. Do not restrict findings to P0/P1 only.
3. Fix its own actionable findings within the authorized scope, add meaningful
   regression coverage where appropriate, and rerun affected checks. The original
   implementation worker must not be assigned these review fixes.
4. Return a compact report: findings with evidence, fixes and files, checks and
   results, unresolved issues, and a readiness verdict. A reviewer that changed code
   cannot provide the final clean verdict for its own fixes.

After any review fix, start another fresh Luna reviewer with the same independent
brief updated for current scope and checks. Review all task changes again. If a
finding needs a product decision or exceeds authorization, surface it to the user
instead of expanding the task. If a finding is demonstrably incorrect, the lead may
reject it with concrete evidence; obtain a fresh review rather than declaring success.

Continue until a fresh reviewer makes no changes, finds no actionable issues, and
judges the implementation ready for production within the checked scope. Require
passing applicable checks and fulfilled acceptance criteria as well as that verdict.
An unavailable required check or unresolved material issue means not ready.
If three consecutive passes make no progress on the same blocker or oscillate between
the same fixes, stop with the evidence and the decision/capability needed to proceed.
Do not call exhaustion a successful review or promise that all bugs are eliminated.

## 5. Report

Summarize the delivered behavior, validation results, clean final review, and any
remaining limits or user verification. Report the selected subagent model and any
host-confirmed model information without inventing it. Distinguish readiness from
deployment: only deploy or perform Git publication when separately authorized.
