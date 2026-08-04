# Audit Checklist Builder

Construct a task-specific checklist from the sections below plus the completion checks of every selected lens. Domain-specific checks live in the lenses; never omit explicit task requirements.

## Universal

- Required artifact exists and is reproducible.
- Every explicit success criterion is tested or proved.
- Assumptions are visible and justified.
- No unresolved dependency is disguised as routine.
- Candidate solves the original task rather than a weakened substitute.
- Candidate meets the delivery floor, not a scaffold, slice, or placeholder.
- Independent validation reproduced the key result.
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
- Tests changed by the implementer reflect intentional product changes.
- No requirement was disabled, mocked, or bypassed to pass validation.
- Duplicate writers, dead branches, stale compatibility paths, and unnecessary indirection introduced during the work were removed.
