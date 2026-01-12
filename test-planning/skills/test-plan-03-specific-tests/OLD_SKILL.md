---
name: test-plan-tco
description: "Use after implementation, before writing tests. Adds the TCO section to TEST_PLAN.md. Enforces checkpoint - user must review before test writing begins."
---
# Test Plan TCO Skill

Use this skill after implementation is complete, before writing test code.

## When to Use

- After implementation code is written
- When the TEST_PLAN.md already has a requirements section (from `test-plan-requirements` skill)
- Before writing any *Tests.swift files

## Workflow

1. **Find the existing test plan**
   - Should already exist with requirements section
   - Location: `{TestsFolder}/{FeatureName}_TEST_PLAN.md`
   - Example: `PizzaCoachTests/OvenAlarm_TEST_PLAN.md`

2. **Write the TCO section**
   - Based on the requirements section AND the implementation just completed
   - Now that code exists, edge cases become visible
   - Use Given/When/Then format for clarity
   - Use equivalence partitioning and boundary testing to identify cases
   - Reference: [identifying-test-cases.md](identifying-test-cases.md)

3. **Consult edge case categories**
   - Timing: early, late, concurrent
   - State: null, already set, partially set
   - User: taps twice, ignores, comes back hours later
   - System: backgrounded, killed, no network
   - Reference `~/.claude/references/EDGE_CASES.md` if it exists

4. **CHECKPOINT - User Approval Required**
   - Present the TCO section to the user
   - Ask: "Ready for your review."
   - **STOP and wait for explicit approval**
   - User fills adversarial reviewer role - looking for gaps
   - Do NOT proceed to test writing without approval

## TCO Format

```markdown
## Test Case Outline

### {Category}

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| {PREFIX}-1 | {Brief description} | {Setup state} | {Action} | {Expected result} |
| {PREFIX}-2 | {Brief description} | {Setup state} | {Action} | {Expected result} |

### {Next Category}

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| ... | ... | ... | ... | ... |

## Implementation Status

- [ ] {PREFIX}-1
- [ ] {PREFIX}-2
```

## After User Approval

Once user approves the TCO:
- Proceed to write tests based on the TCO
- Check off implementation status as tests are written
- If implementation reveals new edge cases, update the TCO and notify user
