---
name: hammer
description: Orchestration framework for difficult problems. Use for substantial work with multiple plausible aproaches, ambiguous requirements, and preservation obligations.
---

# Hammer

Use Hammer to turn an underspecified difficult task into an evidence-backed contract, then orchestrate exploration, implementation, and adversarial validation without silently changing the objective.

## Workflow

1. **Intake**
   - Extract the requested outcome, available artifacts, explicit constraints, and known decisions.
   - Do not ask for information already present in the conversation, files, repository, or connected sources.

2. **Audit before clarification**
   - For an existing codebase, perform a bounded orientation audit before asking questions.
   - Inspect repository structure, docs, build and test commands, likely ownership, analogous implementations, public interfaces, stored formats, and deployment mechanisms.
   - For mathematics or research, audit the exact statement, definitions, supplied sources, permitted background, existing derivations, datasets, experiments, and evidence.
   - Separate unknowns into repository/source-resolvable, user-intent, and mixed unknowns.

3. **Clarification gate**
   - Resolve factual unknowns through inspection.
   - Ask only user-intent questions whose answers would materially change architecture, scope, preservation obligations, risk posture, or completion criteria.
   - Prefer a concise reflected proposal over open-ended questions:

     ```text
     Interpretation: [requested outcome in one or two sentences]
     Delivery floor: [smallest result the user could adopt as the requested outcome]
     Preserve: [user-stated or repo-enforced behavior, interfaces, or guarantees that must not change — never agent-assumed safety chrome such as warning copy, disclosures, or banners]
     Autonomous: [what will be adapted without further approval]
     Needs approval: [objective, acceptance, preservation, cost, safety, external effects]
     Questions: [contract-changing unknowns only, or none]
     ```

   - If immediate execution is explicitly requested, proceed with safe explicit assumptions unless the task would be unsafe or meaningless.

4. **Compile the internal task contract**
   - Read `references/task-contract.md` and instantiate it internally.
   - Keep the contract as the stable orchestration state shared across phases and agents.
   - Amend it only when new evidence requires a change. Record the evidence and invalidate incompatible approaches. Never change it silently.
   - Sequencing, tactics, and internal milestones adapt freely. Crossing the objective, acceptance criteria, preservation obligations, deliverable form (artifact type, language/runtime, host dependencies, interaction model), cost, safety, or external-effect boundary requires renewed user approval recorded as an amendment.
   - The delivery floor is the smallest result the user could adopt as the requested outcome, never the smallest convenient implementation slice. A scaffold, isolated layer, placeholder, or walking skeleton is not a candidate unless explicitly requested.
   - When a run may outlive the session or its context window (compaction), persist the contract, approach registry, and defect ledger to one state file and refresh it at every amendment and synthesis round. Record user-issued operational constraints (required tools, forbidden operations, cost limits) there as preservation obligations. A fresh session or post-compaction continuation resumes from that file plus repository inspection, never from recollection of prior prose.
   - In a git repository, keep state on an orphan branch checked out as a hidden worktree so state history never touches product history: `git worktree add --orphan -b hammer/state .hammer/`, exclude it via `echo '.hammer/' >> .git/info/exclude`, write state to `.hammer/state.md`, and commit on the orphan branch at each refresh. Product commits and state commits never mix. Outside git, fall back to `.ai/hammer-state.md`.
   - One state branch serves every product branch: for multiple runs, give each run its own file `.hammer/runs/<slug>.md` keyed by product branch or task slug, name the run in each state commit, and keep terminal run files as minable history. Never run two concurrent Hammer runs on one product branch.

5. **Select lenses**
   - Choose exactly one primary lens. The primary lens defines what a good solution means and controls the workflow.
   - Add zero or more constraint lenses. They add mandatory checks without replacing the primary lens.
   - Load only the selected files from `lenses/`. If skill, lens, or reference content is already in context, do not re-read it from disk; load each file at most once per contract.
   - Append the primary lens's contract fields to the task contract.

   Primary lenses:
   - `lenses/new-project.md`
   - `lenses/feature-add.md`
   - `lenses/bug-fix.md`
   - `lenses/cleanup.md`
   - `lenses/migration.md`
   - `lenses/mathematics.md`
   - `lenses/research.md`
   - `lenses/system-design.md`

   Constraint lenses:
   - `lenses/security.md`
   - `lenses/performance.md`
   - `lenses/production-readiness.md`
   - `lenses/compatibility.md`
   - `lenses/compliance.md`
   - `lenses/concurrency.md`

6. **Select the smallest sufficient cadence**
   - Run only the phases the material unknowns require. Honor user-directed phases.

   | Condition | Cadence |
   |---|---|
   | Approach accepted and execution requested | implement → validate |
   | Several materially different approaches plausible | explore → decide → implement → validate |
   | Feasibility unresolvable by inspection or questioning | spike → decide → implement → validate |
   | Plan only | clarify → explore → decide |
   | Standalone audit | validate |

   - A spike is a bounded, disposable experiment that retires one material unknown. It never becomes production work by accident.
   - Competitive or resource-bound acceptance (state-of-the-art, beat-X, fixed hardware or budget) is a feasibility unknown until evidence affirms it; an instruction to assume success does not retire it. When a spike cannot affirm feasibility, renegotiate the delivery floor with the user before implementation.
   - Every execution cadence ends with adversarial validation of the integrated result.

7. **Run lens-driven orchestration**
   - Begin with genuinely different approach families when multiple mechanisms are plausible.
   - Preserve independence during early exploration. Do not reveal the favored route to most workers.
   - Brief each worker with a minimal contract slice — objective, invariants, allowed paths, guard command — never a fork of the orchestrator's history. Full-history forks are forbidden for validators, who must not inherit the favored route or prior verdicts, and elsewhere require recorded justification.
   - Maintain an approach registry grouped by underlying mechanism, not wording.
   - Require concrete artifacts: patches, tests, proofs, lemmas, counterexamples, traces, benchmarks, experiments, or precise gaps.
   - Mark routes blocked when their hardest unresolved dependency is equivalent in strength to the original task. Reopen only for a materially new mechanism.
   - Allocate effort by information gain, unresolved risk, and novelty rather than fixed worker counts.
   - Prefer one long or event-driven wait over chains of short polls; polling turns permanently inflate the transcript that every worker brief inherits.
   - Split concurrent writing work by independently verifiable responsibility, not file count. Brief each writing worker with the responsibility map, allowed paths, dependency direction, and the exact check or lint command that guards them.
   - Missing tools, failed approaches, and failing validation are work, not blocks.

8. **Validate adversarially**
   - Read `references/audit-checklists.md` and combine the relevant sections with the selected lens checks.
   - Separate implementation from validation when practical.
   - Validate every candidate against the task contract, not merely against modified tests or the candidate author's interpretation.
   - When the deliverable has a user-facing surface, validate on that surface as the user would — real browser, simulator, device, or a fresh shell running the exact handoff commands. Green suites, health endpoints, and logs are not evidence for surface behavior.
   - Audit the semantic delta: intended changes, preserved behavior, unexplained changes, migrations, rollback, and new runtime prerequisites (config, secrets, keys, service wiring) the change introduces.
   - After integration and after material repairs, simplify: remove duplicate writers, dead branches, stale compatibility paths, and unnecessary indirection, then rerun affected validation.
   - A repair is not closed until its regression passes, affected evidence is rerun, and the relevant adversarial check re-enters. A repair that changes a mechanism reopens simplification and validation.
   - Keep validation proportionate: never redesign a validation protocol or harness twice without executing it against the primary artifact. If consecutive rounds harden verification while the delivery-floor artifact does not advance, run the existing checks or surface a pivot.

9. **Complete truthfully**
   - Declare completion only when the required artifact exists, the contract is satisfied, critical dependencies are resolved, and independent validation reproduces the result.
   - Never end a turn empty: end with verified status plus the next action, or an explicit awaiting-user statement. When a gate blocks an unattended run, record it in the state file with its exact resume command; never pause silently.
   - If the budget is exhausted, return the strongest verified result, exact remaining gap, evidence, invalidated approaches, and highest-value next steps. Never fabricate completion.

## Evidence Policy

Record one policy in the contract. An explicit user policy wins. Default is `open-world`: external documentation, prior art, libraries, mechanisms, and ecosystem constraints are immediately usable. When acceptance requires independent originality or restricted retrieval, use `closed-world-first` or `fact-constrained` as defined in the mathematics and research lenses.

## Internal Contract Usage

Use the contract to:

- brief each worker with only the relevant objective, invariants, scope, and evidence;
- filter proposals that violate preservation requirements or forbidden changes;
- prevent drift across synthesis rounds;
- derive validation cases and completion checks;
- surface important assumptions or contract amendments to the user.

The user normally sees a concise interpretation, not the full internal structure. Surface the full contract when requested, when a breaking tradeoff requires approval, when scope changes materially, or when reporting partial completion.

## Approach Registry

Track at least:

- family and core mechanism;
- concrete result;
- assumptions and dependencies;
- strongest unresolved gap and its strength;
- tested cases and counterevidence;
- state: active, blocked, disproved, merged, or complete;
- exact reopen condition.

Adversarial findings enter the same registry: mechanism, evidence location, regression, and state. Each audit wave diffs against the registry and reports only new or reopened findings; re-finding a defect registered as closed is a repair-closure failure, not a new finding.

Reject status reports and vague optimism. Require falsifiable artifacts and explicit distinctions among established facts, assumptions, hypotheses, and observations.

Rounds completed, workers launched, files changed, and artifacts produced describe execution shape. They never prove progress or support a completion percentage.

## Improving Hammer

If Hammer itself caused waste — a step that burned effort without information, a misleading default, a missing or overweight lens or check — record the exact failure and, after the task completes, open a pull request against `navanchauhan/skills` with the fix so future runs benefit. Propose changes from observed failures, not speculation, and never let this interrupt the task itself.
