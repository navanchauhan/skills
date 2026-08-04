# New Project Lens

**Best solution:** the most coherent complete construction satisfying the full specification.

## Before acting
- Identify users, end-to-end workflows, deployment target, scale, security requirements, production-readiness expectation, and explicit non-goals.
- Audit existing scaffolding and reusable infrastructure.

## Prefer
- End-to-end vertical slices.
- Simple architecture justified by current requirements.
- Integrated components rather than permanent mocks.
- Tests, documentation, observability, deployment, and recovery from the start.

## Avoid
- Speculative abstractions without a current requirement.
- Component shopping without explicit requirement linkage.
- Declaring completion from isolated modules that do not work end to end.

## Completion checks
- Every required workflow works end to end.
- Major components are integrated rather than permanently mocked.
- Failure and operational paths are covered.
- Build, test, run, deploy, and recovery paths are documented.
- Security, configuration, and observability are present.
- Architecture complexity is justified.

## Contract fields
```yaml
users: []
critical_workflows: []
deployment_target: ""
scale_assumptions: []
quality_bar: ""
```
