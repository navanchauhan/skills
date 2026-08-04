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
     Preserve: [behavior, interfaces, or guarantees that must not change]
     Autonomous: [what will be adapted without further approval]
     Needs approval: [objective, acceptance, preservation, cost, safety, external effects]
     Questions: [contract-changing unknowns only, or none]
     ```

   - If immediate execution is explicitly requested, proceed with safe explicit assumptions unless the task would be unsafe or meaningless.

4. **Compile the internal task contract**
   - Read `references/task-contract.md` and instantiate it internally.
   - Keep the contract as the stable orchestration state shared across phases and agents.
   - Amend it only when new evidence requires a change. Record the evidence and invalidate incompatible approaches. Never change it silently.
   - Sequencing, tactics, and internal milestones adapt freely. Crossing the objective, acceptance criteria, preservation obligations, cost, safety, or external-effect boundary requires renewed user approval recorded as an amendment.
   - The delivery floor is the smallest result the user could adopt as the requested outcome, never the smallest convenient implementation slice. A scaffold, isolated layer, placeholder, or walking skeleton is not a candidate unless explicitly requested.
   - When a run may outlive the session, persist the contract and approach registry to one scratch file (for example `.ai/hammer-state.md`) and refresh it at every amendment and synthesis round. A fresh session resumes from that file plus repository inspection, never from recollection of prior prose.

5. **Select lenses**
   - Choose exactly one primary lens. The primary lens defines what a good solution means and controls the workflow.
   - Add zero or more constraint lenses. They add mandatory checks without replacing the primary lens.
   - Load only the selected files from `lenses/`.
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
   - Every execution cadence ends with adversarial validation of the integrated result.

7. **Run lens-driven orchestration**
   - Begin with genuinely different approach families when multiple mechanisms are plausible.
   - Preserve independence during early exploration. Do not reveal the favored route to most workers.
   - Maintain an approach registry grouped by underlying mechanism, not wording.
   - Require concrete artifacts: patches, tests, proofs, lemmas, counterexamples, traces, benchmarks, experiments, or precise gaps.
   - Mark routes blocked when their hardest unresolved dependency is equivalent in strength to the original task. Reopen only for a materially new mechanism.
   - Allocate effort by information gain, unresolved risk, and novelty rather than fixed worker counts.
   - Split concurrent writing work by independently verifiable responsibility, not file count. Brief each writing worker with the responsibility map, allowed paths, dependency direction, and the exact check or lint command that guards them.
   - Missing tools, failed approaches, and failing validation are work, not blocks.

8. **Validate adversarially**
   - Read `references/audit-checklists.md` and combine the relevant sections with the selected lens checks.
   - Separate implementation from validation when practical.
   - Validate every candidate against the task contract, not merely against modified tests or the candidate author's interpretation.
   - Audit the semantic delta: intended changes, preserved behavior, unexplained changes, migrations, and rollback.
   - After integration and after material repairs, simplify: remove duplicate writers, dead branches, stale compatibility paths, and unnecessary indirection, then rerun affected validation.
   - A repair is not closed until its regression passes, affected evidence is rerun, and the relevant adversarial check re-enters. A repair that changes a mechanism reopens simplification and validation.

9. **Complete truthfully**
   - Declare completion only when the required artifact exists, the contract is satisfied, critical dependencies are resolved, and independent validation reproduces the result.
   - If the budget is exhausted, return the strongest verified result, exact remaining gap, evidence, invalidated approaches, and highest-value next steps. Never fabricate completion.

## Evidence Policy

Record one policy in the contract. An explicit user policy wins.

- `open-world` — default for software and product work. External documentation, prior art, libraries, mechanisms, and ecosystem constraints are immediately usable.
- `fact-constrained` — retrieve only specified facts, primary documentation, definitions, or named results. Never search for a solution to the exact task or for its open/solved status.
- `closed-world-first` — default when acceptance requires independent originality. Perform meaningful independent work without retrieval, freeze an independent route or synthesis checkpoint, then review literature for prior art, counterexamples, novelty, and repair. Early failure never opens access.

Label materially retrieval-shaped routes `web-informed` and verify their novelty against literature. Retrieval abstinence neither removes latent knowledge nor proves originality.

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

Reject status reports and vague optimism. Require falsifiable artifacts and explicit distinctions among established facts, assumptions, hypotheses, and observations.

Rounds completed, workers launched, files changed, and artifacts produced describe execution shape. They never prove progress or support a completion percentage.

## Improving Hammer

If Hammer itself caused waste — a step that burned effort without information, a misleading default, a missing or overweight lens or check — record the exact failure and, after the task completes, open a pull request against `navanchauhan/skills` with the fix so future runs benefit. Propose changes from observed failures, not speculation, and never let this interrupt the task itself.
