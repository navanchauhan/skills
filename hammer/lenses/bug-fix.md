# Bug Fix Lens

**Best solution:** the smallest root-cause correction with regression proof.

## Require
- Reproduction, or a documented reason reproduction is impossible.
- A causal mechanism, not only a symptom description.
- A failing regression test before the fix when feasible.
- Search for sibling paths sharing the same cause.
- Passing validation after the fix.

## Avoid
- Broad refactoring during the fix.
- Suppressing symptoms while leaving the cause intact.
- Updating expected behavior solely to match the patch.

## Completion checks
- The regression fails before and passes after.
- Alternate entry points, retries, races, and partial failures are considered.
- Adjacent valid behavior remains unchanged.

## Contract fields
```yaml
reproduction: ""            # or the documented reason reproduction is impossible
suspected_mechanism: ""
affected_surfaces: []
preservation_contract: []
```
