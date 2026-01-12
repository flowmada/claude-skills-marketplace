---
name: test-plan-requirements
description: "Use after planning, before implementation. Creates the requirements section of TEST_PLAN.md. Enforces checkpoint - user must approve before implementation begins."
---
# Test Plan Requirements Skill

Use this skill after completing planning, before writing implementation code.

## When to Use

**Trigger:** When `ExitPlanMode` returns "User has approved your plan"

At that moment, invoke this skill BEFORE:
- Starting to code
- Creating a todo list for implementation
- Any implementation work

Other triggers:
- After writing an ephemeral plan for a new feature
- Before implementing any code
- When starting a new TEST_PLAN.md

## Workflow

1. **Determine test plan location**
   - Place in test folder, adjacent to test files: `{TestsFolder}/{FeatureName}_TEST_PLAN.md`
   - Example: `PizzaCoachTests/OvenAlarm_TEST_PLAN.md`
   - The `{FeatureName}_` prefix ensures unique filenames

2. **Write the requirements section**
   - Feature overview: What it does, key behaviors
   - Key decisions: Link to DECISIONS.md entries if relevant
   - Do NOT write the TCO yet - that comes after implementation

3. **CHECKPOINT - User Approval Required**
   - Present the requirements section to the user
   - Ask: "Does this approach look right?"
   - **STOP and wait for explicit approval**
   - Do NOT proceed to implementation without approval

## TEST_PLAN.md Format (Requirements Section Only)

```markdown
# {FeatureName} - Feature Test Plan

## Feature Overview

{What the feature does, key behaviors, user-facing functionality}

## Key Decisions

See `DECISIONS.md` for full context:
- {Decision 1 summary}
- {Decision 2 summary}

## Test Case Outline

(To be added after implementation)
```

## After User Approval

Once user approves the requirements section:
- Proceed to implementation
- After implementation is complete, invoke the `test-plan-03-specific-tests` skill to add the test cases section
