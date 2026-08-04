# Cleanup Lens

**Best solution:** the largest justified maintainability gain with no intended externally observable behavior change.

## Require
- An explicit behavior boundary.
- Characterization tests where behavior is under-specified.
- Measurable reduction in duplication, coupling, complexity, or inconsistency.
- Incremental, reviewable, reversible transformations.

## Prefer
- Separate mechanical and semantic changes.
- Small commits with one transformation each.
- Evidence that the new abstraction is clearer than the duplication it replaces.

## Avoid
- Mixing cleanup with features.
- Changing public behavior for aesthetic consistency.

## Completion checks
- Error strings, ordering, timing, nil-versus-empty behavior, serialization, performance, and public API shape remain equivalent unless explicitly approved.

## Contract fields
```yaml
behavior_boundary: ""
measured_targets: []        # duplication, coupling, complexity, inconsistency
preservation_contract: []
approved_observable_changes: []
```
