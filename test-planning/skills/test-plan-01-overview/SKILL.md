---
name: test-plan-01-overview
description: "Creates the overview sections of a TEST_PLAN.md by reading the implementation. Use when starting a test plan for existing code. Establishes what we're testing, model structure, and scope before analysis begins."
---
# Test Plan Overview Skill

Creates the foundation of a test plan: what we're testing and how it's structured.

## Orientation (Present to User First)

We're building a test plan together. This is iterative—I present, you review, we refine until it's right.

**What we're creating:** A TEST_PLAN.md with three sections:
- **Overview** — What we're testing, model structure, scope (this skill)
- **Analysis** — How to think about test cases for each part
- **TCO** — The exact test cases, precise enough to implement mechanically

**Your role:** Adversarial reviewer. Look for gaps, push back, catch my mistakes. Each section has a checkpoint—don't approve until you're satisfied.

**Two types of test plans:**
- **Component** — A single unit testable in isolation. All dependencies injectable. Exhaustive testing of boundaries and edge cases.
- **Integration** — Components wired together. Tests correct wiring, state flow, error propagation. Trusts components are already tested.

**The flow:**
1. Confirm Component vs Integration
2. I draft Overview → you review → we iterate
3. Invoke analysis skill → review → iterate
4. Invoke TCO skill → review → iterate
5. Write tests from the TCO

Let's start by confirming what type of test plan this is.

## Workflow

1. **Present orientation and confirm test plan type**
   - Present orientation including:
     - [ ] What we're creating (3 sections)
     - [ ] Your role (adversarial reviewer)
     - [ ] Component — Single unit testable in isolation. All dependencies injectable. Exhaustive boundary testing.
     - [ ] Integration — Components wired together. Tests correct wiring, state flow, error propagation.
     - [ ] The flow (5 steps)
   - State your reasoning for Component vs Integration:
     > "I believe this is a **Component** test plan because [reasoning]. Does that match your intent?"
   - **STOP and wait for user to confirm before proceeding**

### 1a. If Integration type confirmed:

Before drafting overview, check prerequisites:
> "Integration tests assume components are already tested. Let me identify the components involved."

1. List all components this integration touches
2. For each, check if `{Component}_COMPONENT_TEST_PLAN.md` exists in the tests folder
3. If any are missing, tell user:
   > "These components need test plans first: [list]. Want to start with {first missing}?"
4. **Create component plans before integration plan**
5. Only proceed with integration overview after all component plans exist

2. **Read the implementation - CAREFULLY**

   **CRITICAL: Never assume. Always verify.** See [references/never-assume-verify.md](references/never-assume-verify.md)

   - Read the actual source file(s) - not from memory or domain knowledge
   - Identify exact property names, method signatures, dependencies
   - Trace what the code actually calls and depends on
   - Focus on *purpose* and *behavior*, but get the names RIGHT
   - Understand how the pieces fit together

   The Model Structure you write here carries through to Analysis and TCO. Wrong property names here cascade into tests that don't compile or test the wrong thing.

3. **Write What It Does section (skimmable)**
   - One-line summary of what this is
   - Key behaviors in bullets
   - Favor concision over grammar—fragments OK
   - A good test: Can someone skim this in 10 seconds and understand the system?

4. **Write Model Structure section**
   - Inputs: What's set at init / passed in
   - Computed: What we test (functions, properties)
   - This is the list of "testable parts" for later analysis

5. **Write Out of Scope section**
   - What this test plan does NOT cover
   - Purpose: provoke feedback if something should be included
   - Keep it simple—just items and reasons

6. **CHECKPOINT**
   - Present all sections
   - Ask: "Do I understand this correctly? Any gaps in scope?"
   - **STOP and wait for approval before proceeding**

7. **If clarification discussion occurs**
   - If user asks questions or you discuss the overview
   - At conclusion of discussion, offer: "Want me to update the overview based on what we discussed?"
   - Update if user confirms

## Format

```markdown
# {Name} - Component Test Plan
<!-- or: # {Name} - Component Integration Test Plan -->

## What It Does

{One-line summary of what this is.}

- {Key behavior}
- {Key behavior}
- {Edge case handling}

## Model Structure

**Inputs (set at init):**
- {property}: {Type}
- {property}: {Type}

**Computed (what we test):**
- {function/property} → {ReturnType}
- {function/property} → {ReturnType}

## Out of Scope

| Item | Reason |
|------|--------|
| {Excluded item} | {Why excluded} |
```

### Good vs Bad: What It Does

**Bad** (method enumeration):
> State computation (state(now:) -> State)
> - .waiting when sessionStartedAt is nil
> - .mixing when started but mixingCompletedAt is nil

**Good** (skimmable):
> Pure calculation model for poolish-making. Derives current phase from timestamps, provides UI strings.
> 
> - Tracks progress: waiting → mixing → resting → readyForFridge → fermenting
> - Two user actions drive transitions (start session, put in fridge)
> - Time drives rest→ready transition automatically
> - Handles overdue: countdown becomes count-up when late

## After Approval

- **Self-assess**: Complete the Self-Assessment section below
- Create file at `{TestsFolder}/{Name}_COMPONENT_TEST_PLAN.md` or `{Name}_INTEGRATION_TEST_PLAN.md`
- Invoke the `test-plan-02-analysis` skill to continue

## Self-Assessment (After Iteration Complete)

Once the user approves, reflect on the iteration process:

1. **What mistakes did I make?**
   - Misunderstood the code?
   - Missed dependencies or boundaries?
   - Wrong level (Component vs Integration)?

2. **What did the user have to correct?**
   - Gaps in What It Does?
   - Out of scope items I missed?
   - Model structure wrong?

3. **Can this skill be improved?**
   - Direct SKILL.md change (clearer instruction, better format)?
   - Reference documentation needed (examples, patterns, checklists)?

4. **Present analysis to user**
   - Summarize the shortcomings
   - Propose specific improvement(s)
   - User decides whether to update the skill
