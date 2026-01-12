---
name: test-plan-02-analysis
description: "Adds the Analysis section to an existing TEST_PLAN.md. Use after overview is approved. Determines whether formal artifacts are needed or a brief explanation suffices. Works through each testable part from Model Structure."
---
# Test Plan Analysis Skill

Analyzes each testable part before writing test cases. Sometimes that's formal techniques (equivalence classes, boundaries, matrices). Sometimes it's just a clear explanation of what to test.

## Prerequisites

- TEST_PLAN.md exists with approved Requirements, Model Structure, and Out of Scope sections
- Invoke `test-plan-01-overview` skill first if not present

## Workflow

### 0. Read the code under test FIRST

**CRITICAL: See [references/never-assume-verify.md](references/never-assume-verify.md)**

Before doing ANY analysis:
1. Read the actual source file(s) for the code under test
2. Identify exact property names, method signatures, dependencies
3. Trace the data flow - what does the function actually call?

**Never assume based on domain knowledge or memory.** The code is on disk. Read it.

### 1. Read the existing test plan

- Review Model Structure for list of testable parts
- Each "Computed" item needs analysis

### 2. Present analysis ONE PART AT A TIME

**IMPORTANT:** Do not present all parts at once. For each part:
1. Present the analysis for that single part
2. Ask: "Does this analysis look right?"
3. CHECKPOINT - wait for approval
4. Only then move to the next part

This allows focused review and catches issues early.

### 3. For each testable part, assess complexity

Ask: **Would formal analysis reveal something non-obvious?**

**Skip formal artifacts when:**
- Branches are obvious from reading code
- Simple state machine with clear transitions
- Analysis would just restate the code structure
- Single input with straightforward partitions

**Use formal artifacts when:**
- Multiple inputs interact (need matrix)
- Boundaries are subtle (off-by-one risk)
- Complex domain with hidden edge cases
- Format/parsing with many valid/invalid patterns
- Business rules with non-obvious combinations

### 4. Write analysis for each part

**If formal artifacts needed:**
- Equivalence Classes — Partition each input's domain
- Boundary Analysis — Values at, below, above boundaries
- Test Matrix — When dimensions interact
- Domain Analysis — Special values with known significance

**If simple:**
- One-liner explaining what to test
- "Simple: test each state" or "Just nil/non-nil cases"

(CHECKPOINTs happen per-part as described in step 2)

## Format

Use `---` dividers between parts for terminal readability.

```markdown
## Analysis

### {testable part 1}

{One-liner description of what it does}

{Either formal artifacts OR brief explanation of what to test}

---

### {testable part 2}

{One-liner description}

{Formal artifacts OR brief explanation}

---

... repeat for each part from Model Structure ...
```

### Example: Simple (no formal artifacts)

```markdown
### state(now:)

Computes current phase from timestamps. Checked sequentially—first match wins.

Simple: one test per state. Boundary at restElapsed vs counterRestDuration.
```

### Example: Complex (formal artifacts)

```markdown
### waitingCountdown(now:)

Formats time until/since scheduled start.

**Equivalence classes (interval):**
- Within 60s of now
- Future, < 1 hour
- Future, ≥ 1 hour  
- Past, < 1 hour
- Past, ≥ 1 hour

**Boundaries:**
- 59s / 60s / 61s (Now threshold)
- 59m / 60m / 61m (hour display threshold)
```

## Reference

See [references/when-to-use-artifacts.md](references/when-to-use-artifacts.md) for examples of when formal analysis helps vs when it doesn't.

See [references/identifying-test-cases.md](references/identifying-test-cases.md) for detailed guidance on equivalence partitioning and boundary testing techniques.

## After Approval

- **Self-assess**: Complete the Self-Assessment section below
- **Wait for user response** on self-assessment before writing to TEST_PLAN.md
- Write analysis to TEST_PLAN.md
- Invoke the `test-plan-03-specific-tests` skill to continue

## Self-Assessment (After Iteration Complete)

Once the user approves all parts, reflect on the iteration process:

1. **What mistakes did I make?**
   - Forced formal analysis when unnecessary?
   - Skipped analysis that was actually needed?
   - Misidentified equivalence classes or boundaries?

2. **What did the user have to correct?**
   - Gaps in coverage?
   - Unnecessary complexity?
   - Missing edge cases they identified?

3. **Can this skill be improved?**
   - Better criteria for when to use formal artifacts?
   - More examples needed in references?

4. **Present analysis to user and STOP**
   - Summarize the shortcomings
   - Propose specific improvement(s)
   - **Ask: "Do you agree with this self-assessment?"**
   - **STOP and wait for user response before proceeding**
   - User decides whether to update the skill
