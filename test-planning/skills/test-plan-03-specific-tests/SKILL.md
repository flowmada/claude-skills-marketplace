---
name: test-plan-03-specific-tests
description: "Adds the specific test cases for each testable part in TEST_PLAN.md. Use after analysis is approved. Iterates through parts one at a time, presenting analysis + test cases together for review. After approval, test implementation becomes mechanical."
---
# Test Plan TCO Skill

Creates the Test Case Outline—the exact test cases to implement. Works through each part individually, presenting analysis and TCO together for focused review.

## Prerequisites

- TEST_PLAN.md exists with approved Overview and Analysis sections
- Invoke `test-plan-02-analysis` skill first if not present

## Workflow

### 0. Read the code under test FIRST

**CRITICAL: Never assume. Always verify.**

Before writing ANY TCO:
1. Read the actual source file(s) for the code under test
2. Identify exact property names - not what you think they should be
3. Trace what the function actually calls and depends on
4. Verify every Arrange property exists in the code you just read

**The code is on disk. There is zero excuse for guessing.** Using wrong property names means tests won't compile, or worse, test the wrong thing entirely.

See [references/never-assume-verify.md](references/never-assume-verify.md) for the full principle.

### 1. Read the existing test plan

- Identify all testable parts (each has an Analysis subsection)
- You will iterate through these one at a time

### 2. For each part, present Analysis + TCO together

For the current part:
1. Show the Analysis (already written)
2. Write the TCO tables
3. Present both to user
4. CHECKPOINT for this part
5. Move to next part after approval

This keeps context together—user sees reasoning and test cases side by side.

### 3. TCO format requirements

- One heading per test case (or logical group)
- Table with columns: Test Name, Arrange, Act, Assert
- **BLANK ROW between each test** — separates tests visually for readability
- Multi-row Arrange for complex setup (continuation rows leave other columns blank)
- Exact inputs, not descriptions (not "invalid string" but `""`)
- Exact expected outputs (not "throws error" but `throws ValidationError.tooShort`)
- Params not mentioned are nil/default
- Use `t` for reference dates, explain in notes

### 4. TCO Principles

**Before writing any TCO, read [references/tco-principles.md](references/tco-principles.md).**

This file contains principles extracted from TCO reviews:
- Assertion independence (decouple from SUT)
- Variable placement (expected values in Assert, not Arrange)
- Descriptive naming (`originalX` vs `newX`)
- Exhaustive assertions for stateful objects
- Equivalence classes for numeric inputs

As we collaborate on more TCOs, new principles get added there.

### 5. Collection assertion decisions

When a function returns an ordered collection (array, set, sequence):
- **Consider exhaustive vs selective assertions** - should tests verify the entire collection equals an expected value, or just check for presence of certain elements?
- **Exhaustive is usually better** - it catches unexpected extra elements and ordering issues
- **Cumulative tests for building collections** - if tests add elements progressively (0, 1, 2...), each test should verify the exact collection state

**You must explicitly state your decision and ask:** "I'm using [exhaustive/selective] assertions for [collection]. Do you agree that is the best approach?"

Example (exhaustive):
```
| `completedSteps_2` | timestamps for steps 0,1 | `completedSteps` | `== [.step0, .step1]` |
```

### 6. Make assertions meaningful with proper context

Tests should prove the result comes from the specific condition being tested, not some other factor.

**For positive assertions — verify preconditions:**
When an assertion depends on a relationship or condition, add a precondition assertion proving that condition holds.

Example: Testing that item at index 3 is highlighted because it equals `selectedIndex`:
```
| precondition: `selectedIndex == 3` | | |
| `item_atSelectedIndex_isHighlighted` | `selectedIndex: 3` | `isHighlighted(index: 3)` | `true` |
```
The precondition proves the highlight comes from the selected-index relationship, not coincidence.

**For nil/empty assertions — arrange the inverse for context:**

When testing a nil or empty result for a specific reason, arrange the opposite for other aspects:
- Testing nil? Arrange non-nil values for other data.
- Testing empty collection? Arrange non-empty collections for other data.

This proves the result is due to the specific condition being tested, not just empty state everywhere.

Example (nil): Testing `getEmail(for: .guest)` returns nil because guests have no email:
```
BAD:  all user data nil → nil (proves nothing)
GOOD: admin.email and member.email are non-nil, guest.email is nil → nil (proves guests specifically lack email)
```

Example (empty): Testing `getPermissions(for: .guest)` returns empty because guests have no permissions:
```
BAD:  all permission lists empty → [] (proves nothing)
GOOD: admin and member have non-empty permission lists, guest has none → [] (proves guests specifically lack permissions)
```

### 7. CHECKPOINT (per part)

Present the part:
```
## {part name}

### Analysis
{existing analysis}

### TCO
{new tables}
```

Ask: "Does this TCO match the analysis? Any missing cases?"

**STOP and wait for approval before moving to next part**

### 8. If clarification discussion occurs

If user asks questions or you discuss the TCO:
- At conclusion of discussion, offer: "Want me to update the TCO based on what we discussed?"
- Update if user confirms

### 9. After all parts complete

- Add Notes section (conventions like `t` = reference date)

## Format

```markdown
## {testable part}

### Analysis

{existing analysis from previous skill}

### TCO

#### {test case group}

| Test Name | Arrange | Act | Assert |
|-----------|---------|-----|--------|
| `{test_name_1}` | `{param}: {value}` | `{method call}` | `{expected}` |
| | `{param}: {value}` | | |
| | | | |
| `{test_name_2}` | `{param}: {value}` | `{method call}` | `{expected}` |
| | `{param}: {value}` | | |
| | | | |
| `{test_name_3}` | `{param}: {value}` | `{method call}` | `{expected}` |

---

## {next testable part}

... repeat ...

---

**Notes:**
- Params not mentioned are nil/default
- `t` is any reference date
```

## Example Presentation (One Part)

When presenting a part in terminal, show:

```
## state(now:)

### Analysis

Computes current phase from timestamps. Checked sequentially—first match wins.

Simple: one test per state. Boundary at `restElapsed` vs `counterRestDuration`.

### TCO

#### .fermenting

| Test Name | Arrange | Act | Assert |
|-----------|---------|-----|--------|
| `state_whenIntoFridgeAtSet_returnsFermenting` | `intoFridgeAt: t` | `state(now: t)` | `.fermenting` |

#### .waiting

| Test Name | Arrange | Act | Assert |
|-----------|---------|-----|--------|
| `state_whenNoTimestamps_returnsWaiting` | all timestamps nil | `state(now: t)` | `.waiting` |

... etc ...

---

Does this TCO match the analysis? Any missing cases?
```

## Precision Checklist

Before presenting each part, verify:
- [ ] Read [references/tco-principles.md](references/tco-principles.md)
- [ ] Every equivalence class from Analysis has test case(s)
- [ ] Every boundary has test cases (at, below, above where relevant)
- [ ] Inputs are exact values, not descriptions
- [ ] Expected outputs are exact, including error types
- [ ] Multi-row arrange is readable (other columns blank on continuation rows)

## After All Parts Approved

- **Self-assess**: Complete the Self-Assessment section below
- Write tests mechanically from the TCO
- Test file maps 1:1 with the test plan

## Self-Assessment (After Iteration Complete)

Once all parts are approved, reflect on the iteration process:

1. **What mistakes did I make?**
   - Test cases missing from analysis?
   - Imprecise inputs or outputs?
   - Table format issues?

2. **What did the user have to correct?**
   - Missing test cases?
   - Incorrect expected values?
   - Readability issues?

3. **Can this skill be improved?**
   - Direct SKILL.md change (clearer instruction, better format)?
   - Reference documentation needed (more examples)?

4. **Present analysis to user and STOP**
   - Summarize the shortcomings
   - Propose specific improvement(s)
   - **Ask: "Do you agree with this self-assessment?"**
   - **STOP and wait for user response before proceeding**
   - User decides whether to update the skill
