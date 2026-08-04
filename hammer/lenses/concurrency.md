# Concurrency Constraint Lens

Add these requirements to the primary lens:

- State ordering, atomicity, consistency, and cancellation assumptions.
- Analyze races, deadlocks, starvation, retries, duplicate delivery, split-brain, and partial failure.
- Cover crashes between side effects and idempotency requirements.
- Build deterministic or stress-based reproduction where possible.
- Validate timeout, shutdown, backpressure, and stale-read behavior.
