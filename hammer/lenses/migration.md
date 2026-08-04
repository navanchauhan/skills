# Migration Lens

**Best solution:** the safest observable and reversible transition between old and new states.

## Require
- Explicit old and new state definitions.
- Compatibility matrix.
- Staged rollout with checkpoints.
- Idempotent and resumable operations.
- Mixed-version behavior.
- Observability, rollback criteria, and data recovery.

## Never assume
- Atomic fleet-wide transition.
- One-time execution without retries.
- Immediate removal of old readers or writers.

## Prefer
- The expand → compatible readers and writers → backfill and verify → cut over → observe → contract ordering.

## Completion checks
- Interrupted migration, duplicate execution, downgrade, partial conversion, and rollback-after-write scenarios are tested.

## Contract fields
```yaml
old_state: ""
new_state: ""
compatibility_matrix: []
rollout_stages: []
rollback_criteria: []
preservation_contract: []
```
