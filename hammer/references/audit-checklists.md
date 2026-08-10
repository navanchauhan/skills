# Audit Checklist Builder

Construct a task-specific checklist from the sections below plus the completion checks of every selected lens. Domain-specific checks live in the lenses; never omit explicit task requirements.

## Universal

- Required artifact exists and is reproducible.
- Every explicit success criterion is tested or proved.
- Assumptions are visible and justified.
- No unresolved dependency is disguised as routine.
- Candidate solves the original task rather than a weakened substitute.
- Each acceptance criterion individually meets the delivery floor, classified full, degraded, or placeholder; degraded and placeholder items are named to the user, never passed inside an aggregate claim.
- Independent validation reproduced the key result.
- When the deliverable has a user-facing surface, validation exercised it as the user would — real browser, simulator, device, or a fresh shell running the exact handoff commands — against the integrated system. A surface may be deferred as an external gate only when no in-environment substitute exists.
- Claims about live system state (reachability, deploy health) were re-observed during this pass; earlier observations are hypotheses, not evidence.
- Each user-reported defect was confirmed fixed on the surface the user reported it from.
- Every repair has a passing regression, rerun affected evidence, and a re-entered adversarial check.
- Progress metrics such as rounds, workers, commits, or files changed were not treated as completion evidence.

## Existing Software

- Current behavior was characterized before change.
- Intended semantic changes are explicit.
- Unrelated behavior is preserved.
- Public APIs, schemas, protocols, and stored formats are reviewed.
- Callers, integrations, and mixed versions are considered.
- Error behavior, ordering, retries, timing, and side effects are checked.
- Migration, rollback, and partial deployment are handled.
- New runtime prerequisites introduced by the change are provisioned on every deployment target before cutover.
- Deploy pre-flight: artifact platform and architecture match the target host; expensive builds stay off constrained production hosts.
- Tests changed by the implementer reflect intentional product changes.
- No requirement was disabled, mocked, or bypassed to pass validation.
- Duplicate writers, dead branches, stale compatibility paths, and unnecessary indirection introduced during the work were removed.
