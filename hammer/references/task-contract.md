# Task Contract Template

Use internally before broad orchestration. Append the contract fields declared by the selected primary lens. Constraint lenses add requirements and checks, not fields.

```yaml
objective: ""
deliverable: ""             # include form: language/runtime, self-contained vs host-dependent, interaction model
delivery_floor: ""          # smallest result the user could adopt as the requested outcome
primary_lens: ""
constraint_lenses: []
cadence: ""
evidence_policy: ""         # open-world | fact-constrained | closed-world-first

success_criteria: []
non_goals: []
constraints: []

adaptation_boundary:
  autonomous: []            # sequencing, tactics, internal milestones
  requires_approval: []     # objective, acceptance, preservation, deliverable form or runtime dependencies, cost, safety, external effects

validation_requirements: []
explicit_assumptions: []
blocking_questions: []
non_blocking_unknowns: []

execution_budget:
  concurrency: ""
  rounds: ""
  effort_tiers: ""          # orchestration and synthesis highest; ordinary workers one tier down; mechanical fan-out cheapest
  other_limits: []
```

A blocking question remains only when plausible answers materially change execution. Resolve repository facts through inspection, not user questioning.
