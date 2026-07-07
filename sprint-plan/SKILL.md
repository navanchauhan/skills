---
name: sprint-plan
description: Create sprint plan by orchestrating subagents and external coding agents
---

# Sprint Plan

Run this skill directly in the current session. You are the orchestrator — create and update state files with file tools, dispatch subagents and external agents, verify artifacts, and persist the result. This skill is also used to queue up additional sprints while another agent is working on executing a sprint.

```
availability → 1 state → 2 orient → 3 intent → 4 drafts → 5 critiques → 6 reviews → 7 interview → 8 merge → 9 gate → 10 persist
```

Numbers map to the stage sections below. Working files use `SPRINT_<NNN>_*` (underscore); the persisted final sprint uses `SPRINT-<NNN>` (hyphen).

## Roles

- **Orchestrator (you)**: runs all stages, writes state files, dispatches agents, resolves conflicts, runs the quality gate, persists the final sprint
- **Subagents**: handle orient and intent work — repo mapping, test/release discovery, risks, architecture/implementation/delivery intent
- **External coding agents**: draft, critique, and review. Skip basic file management in their prompts.

## State Files

```
.ai/sprint-state/current.env
.ai/sprint-state/history.log
.ai/sprint-state/queue.tsv       (optional index; sprint files remain canonical)
.ai/sprint-state/ORIENT_REPO.md
.ai/sprint-state/ORIENT_TESTS.md
.ai/sprint-state/ORIENT_RISKS.md
.ai/sprint-state/INTENT_ARCHITECTURE.md
.ai/sprint-state/INTENT_IMPLEMENTATION.md
.ai/sprint-state/INTENT_DELIVERY.md
.ai/sprint-state/INTENT_SYNTHESIS.md
.ai/sprint-state/INTERVIEW_ASSUMPTIONS.md    (if needed)
.ai/sprint-state/final-sprint.md
.ai/sprints/SPRINT-<NNN>.md
```

`.ai/sprints/SPRINT-*.md` is the canonical sprint queue. Multiple planned sprints may exist while another sprint is being implemented. Old sprint docs are historical input and queued work items; do not rely on a legacy ledger format and do not delete or overwrite queued sprint files just because they are not the active execution target.

`current.env` is a convenience pointer to the sprint currently being planned or most recently planned by this skill. It is not a global singleton queue and must not be used to erase, supersede, or reinterpret other `.ai/sprints/SPRINT-*.md` files. Each new planning run overwrites it in full (see Stage 1), so at any moment it reflects at most one sprint — consumers must check `SPRINT_ID` before trusting `SPRINT_STATUS`, and its absence or staleness never means a sprint doesn't exist or isn't in progress elsewhere.

`current.env`:
```
SPRINT_ID=<NNN>
SPRINT_TITLE=<title>
SPRINT_STATUS=planning|planned|implementing|implemented|blocked
SPRINT_CREATED_AT=<UTC ISO-8601>
SPRINT_UPDATED_AT=<UTC ISO-8601>
```

`history.log` — append-only, tab-separated:
```
<UTC ISO-8601>	<NNN>	<event>	<detail>
```

If maintained, `queue.tsv` is append/update metadata only:
```
<NNN>	<status>	<source>	<title>	<UTC ISO-8601 updated>
```

The queue status may mirror `planning`, `planned`, `implementing`, `implemented`, or `blocked`. `sprint-plan` writes the `planned` row at persist time; `sprint-execute` updates the same row for `implementing`, `implemented`, and `blocked` transitions (see sprint-execute Stage 7). Missing `queue.tsv` entries do not invalidate sprint files.

## Agent Availability

Check each command individually and record results in `.ai/coding-agents.md`:

```sh
agy --version
claude --version
codex --version
```

Currently we support Claude Code, Codex, and Antigravity-CLI only.

## Stage 1: Create Sprint State

Inspect all known sprint identifiers to determine the next sprint ID: `.ai/sprints/SPRINT-*.md`, `.ai/sprints/SPRINT_<NNN>_*` drafts, `.ai/sprint-state/current.env`, `.ai/sprint-state/history.log`, `.ai/sprint-state/queue.tsv` if present, `.ai/sprint-execution/current.env`, and `.ai/sprint-execution/progress.log` if present. Choose one higher than the maximum ID found; never reuse an ID.

Write `current.env` for the new planning run, overwriting any prior pointer contents, and append a `planning` event to `history.log`. This overwrite is expected even if another sprint is mid-execution (see the note on `current.env` above — it tracks only the latest planning action, not overall queue state). If `.ai/sprint-execution/current.env` shows another sprint is `implementing`, leave execution state untouched. Planning a new sprint while another sprint executes is valid; the existing sprint remains in the queue/execution state and the new sprint becomes an additional queued item when persisted.

## Stage 2: Orient

Use subagents to cover: repo structure, existing sprint history, test/CI conventions, known TODOs and risk hotspots, and agent availability. Write `ORIENT_REPO.md`, `ORIENT_TESTS.md`, `ORIENT_RISKS.md`.

## Stage 3: Intent

Use subagents to generate intent options from orientation evidence, then synthesize. Each intent file should define objective, non-goals, success criteria, implementation lanes, validation strategy, and likely agent allocation. Write `INTENT_ARCHITECTURE.md`, `INTENT_IMPLEMENTATION.md`, `INTENT_DELIVERY.md`, `INTENT_SYNTHESIS.md`.

## Stage 4: External Drafts

Each available external agent produces an independent draft. Prompts must name the output file and ask the agent to read `INTENT_SYNTHESIS.md` and `.ai/coding-agents.md`. Don't replace external drafts unless no external agents are available.

Invocations:
```sh
claude --dangerously-skip-permissions --model fable --print "$prompt" # fallback to opus if unavailable
codex exec --yolo "$prompt"
agy --dangerously-skip-permissions --print "$prompt"
```

Draft files:
```
.ai/sprints/SPRINT_<NNN>_DRAFT_CLAUDE.md        (architecture-focused)
.ai/sprints/SPRINT_<NNN>_DRAFT_CODEX.md         (implementation-focused)
.ai/sprints/SPRINT_<NNN>_DRAFT_ANTIGRAVITY.md   (delivery-focused)
```

Wait policy:
- Do not treat lack of stdout as failure. External coding agents may produce no terminal output until they finish.
- For external coding agents, allow up to 30 minutes per invocation before considering it timed out. Poll the process without interrupting it.
- An invocation fails only if the process exits non-zero, the expected artifact is still missing/empty after the process exits, or the wait budget is exhausted.

After each dispatch, verify the file exists and is non-empty. If an agent fails or returns empty, retry once with the same wait policy; if it still fails, drop that lane and note it in the merge.

## Stage 5: External Critiques

Each available agent critiques the other agents' drafts (not its own). Critiques must flag: vague or missing tasks, missing validation, unrealistic sequencing, hidden dependencies, unbounded scope, missing closeout criteria, unjustified agent usage, and actionable fixes.

Artifact pattern:
```
.ai/sprints/SPRINT_<NNN>_<CRITIC>_CRITIQUE_<AUTHOR>.md
```

Use the Stage 4 wait policy for every external critique invocation. Do not create orchestrator fallback critique files until the relevant agent has exited/failed or exceeded the wait budget and one retry has also failed.

## Stage 6: External Critique Reviews

Each available agent reviews critiques — judging whether critique points are valid, overstated, missing key issues, or important for merge.

Artifact pattern:
```
.ai/sprints/SPRINT_<NNN>_<REVIEWER>_REVIEW_<CRITIC>_CRITIQUE_<AUTHOR>.md
```
e.g. `SPRINT_<NNN>_CODEX_REVIEW_CLAUDE_CRITIQUE_ANTIGRAVITY.md` — Codex reviewing Claude's critique of Antigravity's draft.

Verify each file as in Stage 4, using the same wait policy. Do not create orchestrator fallback review files until the relevant agent has exited/failed or exceeded the wait budget and one retry has also failed.

## Stage 7: Interview (skip if no ambiguity)

If material ambiguity remains, ask the user concise questions — only those that change the final plan. Prefer asking the user questions unless they requested to make the best assumptions possible, in which case, record assumptions in `.ai/sprint-state/INTERVIEW_ASSUMPTIONS.md`.

## Stage 8: Orchestrator Merge

Read all orientation, intent, draft, critique, critique-review, and interview artifacts. Produce `.ai/sprint-state/final-sprint.md`. Keep only decisions that survived critique. Resolve conflicts explicitly.

## Stage 9: Quality Gate

The final plan must include: concrete objective and non-goals, current-state evidence, agent availability summary, scope, phased implementation steps, touched files/modules, validation commands, risk register, fallback plan, definition of done, and closeout checklist. Revise before persisting if any are missing.

## Stage 10: Persist

Write the full contents of `.ai/sprint-state/final-sprint.md` to `.ai/sprints/SPRINT-<NNN>.md`. The persisted sprint file is the immutable historical sprint artifact and must be self-contained; do not make it a pointer, symlink, stub, or "see final-sprint.md" wrapper. `final-sprint.md` is only the current merge/staging output and may change in later planning work, so it cannot be the canonical record for queued or historical sprints.

After writing, verify `.ai/sprints/SPRINT-<NNN>.md` contains the complete final sprint template sections and is not pointer-only. Update `current.env` with `SPRINT_STATUS=planned` and current timestamp for this sprint only. Append a `planned` event to `history.log`. If using `queue.tsv`, add or update the row for this sprint as `planned`.

Do not remove, rewrite, quarantine, or mark implemented any other `.ai/sprints/SPRINT-*.md` or `.ai/sprints/SPRINT_<NNN>_*` artifacts during persistence. Existing future sprints are first-class queued work, even if a different sprint is currently executing. If an external agent creates unexpected sprint artifacts for a different ID during this planning run, leave them in place, record them in `.ai/sprint-state/UNEXPECTED_ARTIFACTS.md`, and ask before deleting or merging them.

## Final Sprint Template

```markdown
# SPRINT-<NNN>: <title>

## Objective

## Current State Evidence

## Coding Agent Availability

## Scope

## Non-Goals

## Implementation Plan

## File and Module Map

## Validation Plan

## Risks and Fallbacks

## Definition of Done

## Closeout Checklist
```
