# Feature Add Lens

**Best solution:** the smallest semantically complete extension that preserves all unrelated behavior.

## Before acting
- Map current behavior, ownership, callers, analogous features, tests, public interfaces, stored formats, deployment behavior, and hidden compatibility surfaces.
- Establish explicit change and preservation contracts.
- Add characterization tests for important unprotected behavior.

## Prefer
- Extension over replacement.
- Local changes over global refactors.
- Existing abstractions and conventions.
- Explicit adapters or migrations for unavoidable incompatibility.
- The contract and boundary cases → domain and state → adapters and API → user surface → end-to-end evidence ordering.

## Forbid by default
- Unrelated cleanup or broad rewrites.
- Weakening requirements to pass tests.
- Changing tests merely to accommodate the implementation.

## Completion checks
- Intentional and unintentional semantic differences are identified.
- Downstream callers, stored data, errors, ordering, retries, timing, and serialization are audited.
- Migration and rollback are validated.
- The patch cannot be made materially smaller without losing correctness.

## Contract fields
```yaml
current_state:
  summary: ""
  relevant_components: []
  callers_and_consumers: []
  compatibility_surfaces: []
change_contract: []
preservation_contract: []
forbidden_changes: []
```
