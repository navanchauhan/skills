# System Design Lens

**Best solution:** a design whose requirements, interfaces, ownership, failure paths, and tradeoffs are explicit and defensible.

## Require
- Functional and non-functional requirements.
- Constraints and scale assumptions.
- Data and control flows.
- Ownership and trust boundaries.
- Failure, recovery, deployment, observability, and operational lifecycle.
- Alternatives compared against explicit criteria.

## Reject
- Unsupported scalability claims.
- Component selection without requirement linkage.
- Happy-path-only architecture.

## Completion checks
- Major tradeoffs and rejected alternatives are documented.
- Failure and recovery paths are concrete.
- Interfaces and operational ownership are unambiguous.

## Contract fields
```yaml
functional_requirements: []
non_functional_requirements: []
scale_assumptions: []
decision_criteria: []       # what makes one alternative win
```
