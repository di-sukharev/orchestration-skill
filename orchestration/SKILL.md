---
name: orchestration
description: Plan and implement a feature through a lead, compact Luna discovery, tiny sequential tasks for a reused Luna worker, and fresh reviewers who fix findings agreed by the lead. Use when the user requests this delegated implementation workflow or invokes orchestration.
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
  Start new agents without parent history (`fork_turns: "none"` or equivalent).
  Reuse the implementation worker through follow-up messages by default; fresh
  reviewers are mandatory on every review pass.
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

## Communication boundary

Never read subagent conversation histories, transcripts, or internal context, even
to recover a missing result. Communicate through explicit task briefs and compact
reports only: JSON or equally short free-form text matching the requested report
fields. Ask for missing evidence with a targeted follow-up, not a history dump.
The lead may inspect repository code, diffs, and focused check output directly.

Give enough direction to make each assignment precise, but do not supply code,
patches, or line-by-line pseudocode. Describe the intended behavior, owning layer,
high-level implementation idea, constraints, and relevant pitfalls. The subagent
chooses and writes the actual implementation.

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

State the intended product behavior and split the plan into very small subtasks
in dependency order. Each should have one narrow, verifiable outcome; keep coupled
edits together only when needed to make that outcome coherent. Do not hand the
worker a whole feature or a batch to execute autonomously. Resolve material product
ambiguity with the user; make routine engineering decisions yourself.

Before assigning each subtask, define its Definition of Done (DoD):

- A stable task ID, the exact behavior to deliver, and scope/non-goals.
- Useful paths, the high-level implementation idea, and decisions already made.
- Relevant edge cases, invariants, and pitfalls to account for.
- Observable acceptance criteria and the smallest appropriate checks proving them.
- The compact report expected before the next assignment.

## 3. Implement one tiny subtask at a time

Start one Luna worker and retain its agent ID. You may reuse the discovery scout
as the worker if its context is useful and the host supports continuation. Send
exactly one subtask with its DoD, wait for its report, and verify completion before
sending the next subtask to the same agent. Reuse its knowledge of the implementation
instead of forcing every new worker to rediscover the project.

The worker implements only its current assignment and runs its focused checks.
For a reproducible behavior bug, ask for a failing regression test before the fix
when existing infrastructure supports it. It must not pick the next task itself.

Require a short completion report, for example:

```json
{
  "task": "T1",
  "status": "done",
  "changes": [{"path": "src/example.ts", "effect": "Behavior delivered"}],
  "dod": [{"criterion": "Expected outcome", "evidence": "Check or observation"}],
  "checks": [{"command": "project test command", "result": "passed"}],
  "open": []
}
```

Use `blocked` or `incomplete` when appropriate, and put failures, missing checks,
deviations, and questions in `open`. Free-form reports with the same facts are fine.
Do not send source dumps or full logs, and do not label unrun checks as passing.

The lead compares the evidence with the DoD and inspects the relevant diff as needed.
Return incomplete work to the same worker with precise corrections. Start the next
subtask only after the current DoD is met. Keep prompts complete enough to explain
the required outcome from start to finish, without prescribing the implementation
as code or expanding the assignment.

The lead may replace the worker with a fresh Luna agent when independent judgment
would reduce bias, context has become stale, the area changes substantially, or
continuation is unavailable. Provide a compact handoff with current decisions,
paths, completed outcomes, and the next DoD; never transfer or read agent histories.

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

Each review pass has two stages:

1. **Investigate and report, without edits.** The fresh reviewer independently
   inspects the combined changes and surrounding behavior. It returns a compact
   list of substantiated bugs, missed requirements, regressions, bottlenecks, and
   material maintainability problems. Each finding needs an ID, location, concrete
   failure/impact, evidence or uncertainty, and a suggested verification. Avoid
   speculative style preferences; do not restrict findings to P0/P1 only. Report
   checks, remaining limitations, and a readiness verdict when there are no issues.
2. **Lead decision, then fixes by the same reviewer.** The lead assesses the findings
   and sends specific instructions about what to verify and how, which findings
   are accepted, and the intended correction with its DoD. For an uncertain finding,
   ask the reviewer for a bounded read-only check first, then decide. The reviewer
   edits only after the lead agrees, fixes the accepted issues, adds meaningful
   regression coverage where appropriate, and reruns affected checks. Keep these
   assignments small and sequential too. It reports fixes and DoD evidence through
   the same compact protocol. Do not assign review fixes to the original worker.

Agreement here is the lead's engineering decision within the user's existing
authorization, not a new user approval gate. A reviewer that changed code cannot
provide the final clean verdict for its own fixes.

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
