# test-plan-02-analysis

Phase 2 of 3 in the test planning workflow. Analyzes each testable part before writing test cases.

## Sequence

test-plan-01-overview → **test-plan-02-analysis** → test-plan-03-specific-tests

## Trigger

Automatically invoked after overview is approved.

## What It Creates

For each testable part from Model Structure:
- **Simple cases:** One-liner explanation ("test each state")
- **Complex cases:** Formal artifacts (equivalence classes, boundaries, test matrix)

## Checkpoint

Presents parts ONE AT A TIME. Each requires approval before moving to next.

## Reference Files

- `references/when-to-use-artifacts.md`
- `references/identifying-test-cases.md`
- `references/never-assume-verify.md`
