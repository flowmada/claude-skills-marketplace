---
name: test-plan-analysis
description: "Adds the Analysis section for each testable part in TEST_PLAN.md. Use after overview is approved. Determines whether formal artifacts are needed or a brief explanation suffices. Works through each testable part from Model Structure."
---
# Test Plan Analysis Skill

Analyzes each testable part before writing test cases. Sometimes that's formal techniques (equivalence classes, boundaries, matrices). Sometimes it's just a clear explanation of what to test.

## Prerequisites

- TEST_PLAN.md exists with approved What It Does, Model Structure, and Out of Scope sections
- Invoke `test-plan-01-overview` skill first if not present

## Workflow

### 1. Read the existing test plan

- Review Model Structure for list of testable parts
- Each "Computed" item needs analysis

### 2. For each testable part, assess complexity

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

### 3. Write analysis for each part

Create a section for each testable part with Analysis subsection.

**If formal artifacts needed:**
- Equivalence Classes — Partition each input's domain
- Boundary Analysis — Values at, below, above boundaries
- Test Matrix — When dimensions interact
- Boundary testing rationale — Explain why specific values chosen

**If simple:**
- One-liner explaining what to test
- "Simple: test each state" or "Just nil/non-nil cases"

### 4. CHECKPOINT

Present analysis for all parts.

Ask: "Is this analysis correct? Any gaps?"

**STOP and wait for approval before proceeding**

### 5. If clarification discussion occurs

If user asks questions or you discuss the analysis:
- At conclusion of discussion, offer: "Want me to update the analysis based on what we discussed?"
- Update if user confirms

## Format

```markdown
## state(now:)

### Analysis

{One-liner description of what it does}

{Either formal artifacts OR brief explanation of what to test}

---

## isComplete

### Analysis

{One-liner description}

{Formal artifacts OR brief explanation}

---

... repeat for each part from Model Structure ...
```

### Example: Simple (no formal artifacts)

```markdown
## state(now:)

### Analysis

Computes current phase from timestamps. Checked sequentially—first match wins.

Simple: one test per state. Boundary at `restElapsed` vs `counterRestDuration`.
```

### Example: Complex (formal artifacts)

```markdown
## waitingCountdown(now:)

### Analysis

Formats time until/since scheduled start.

**Equivalence classes (interval):**
- Within 60s of scheduled → "Now"
- Future, < 1 hour → "in Xm"
- Future, ≥ 1 hour → "in Xh Ym"
- Past, < 1 hour → "Xm ago"
- Past, ≥ 1 hour → "Xh ago"

**Boundaries:**
- 59s / 60s (Now threshold)
- 59m / 60m (hour display)

*Boundary testing rationale:*
- 59 = last value where `< 60` is true → "Now" (below)
- 60 = first value where `< 60` is false → "in 1m" (at)
- 61 is redundant — same equivalence class as 60
```

## Reference

See [references/when-to-use-artifacts.md](references/when-to-use-artifacts.md) for examples of when formal analysis helps vs when it doesn't.

See [references/identifying-test-cases.md](references/identifying-test-cases.md) for detailed guidance on equivalence partitioning and boundary testing techniques.

## After Approval

- **Self-assess**: Complete the Self-Assessment section below
- Invoke the `test-plan-03-specific-tests` skill to continue

## Self-Assessment (After Iteration Complete)

Once the user approves, reflect on the iteration process:

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

4. **Present analysis to user**
   - Summarize the shortcomings
   - Propose specific improvement(s)
   - User decides whether to update the skill
