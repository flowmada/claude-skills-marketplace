# TCO Principles

Principles extracted from TCO reviews. Read before presenting each part.

---

## Assertion Independence

**Assertions must be decoupled from the system under test.** If Arrange or Act changes, the assertion should *fail* until you explicitly update the expected value—not magically pass because it recomputed the "right" answer.

**Define expected values separately from the SUT:**
- Hard-code expected values, OR
- Compute expected values from other controlled values you define (not from the SUT)

**Good:**
```
Arrange: originalCookTime = Date(ts: 0), preheatDuration: 3600
Assert:  newCookTime = Date(ts: 900)  // hard-coded, defined in Assert
         .ovenStart == newCookTime.adding(-3600)  // offset matches preheatDuration
```

**Bad:**
```
Assert: .ovenStart == schedule.cookTime.adding(-preheatDuration)
        // Computes from SUT—would magically update if schedule changes
```

**Why this matters:**
- Reviewer can verify expected values match Arrange (e.g., `-3600` matches `preheatDuration: 3600`)
- Changing Arrange forces explicit update to Assert
- No hidden coupling between test setup and expected results

---

## Variable Placement

Expected values belong in Assert column, not Arrange.

- **Arrange** = setup (inputs to SUT)
- **Assert** = expectations (hard-coded or computed from controlled values)

**Good:**
```
| Arrange | Act | Assert |
| schedule = Schedule(cookTime: t) | schedule.adjust(by: 900) | newCookTime = Date(ts: 900) |
|                                  |                          | .cookTime == newCookTime    |
```

**Bad:**
```
| Arrange | Act | Assert |
| schedule = Schedule(cookTime: t) |                          |                             |
| newCookTime = Date(ts: 900)      | schedule.adjust(by: 900) | .cookTime == newCookTime    |
```

---

## Descriptive Naming

Use names that clarify intent and relationships.

**Good:** `originalCookTime` vs `newCookTime`
**Bad:** `t` vs `t2`

For unchanged vs changed values:
- Express unchanged values relative to `originalX`
- Express changed values relative to `newX`
- Add `// unchanged` comments for clarity

---

## Intent + Safety Net for Mutating Methods

When testing methods that mutate state, use two parts:

1. **Intent assertions** (one or more) — Explicitly test expected changes with readable calculations
2. **Safety net assertion** (one) — Compare full state to catch unintended side effects

**Pattern:**
```swift
// Intent - documents what we're testing
let newCookTime = stateBefore.cookTime.addingTimeInterval(900)
XCTAssertEqual(state.cookTime, newCookTime)

// Safety net - catches unintended side effects
var expectedState = stateBefore
expectedState.cookTime = newCookTime
XCTAssertEqual(state, expectedState)
```

**Why both:**
- Intent assertions are readable — show the calculation, document purpose
- Safety net is exhaustive — catches accidental changes to other properties
- They check different things — one could fail while other passes

**Requires:** Object must be Equatable.

---

## Mutation Guard for Non-Mutating Methods

Non-mutating methods return values without side effects. Add one test per method verifying no mutation occurred.

**When:** Method is intended to be non-mutating (reads state, returns computed value).

**Pattern:**
```swift
let stateBefore = state
_ = state.queryMethod(at: now)
XCTAssertEqual(state, stateBefore)
```

**Why:** Regression guard. If someone accidentally adds mutation later, test fails.

**Setup strategy:**
- **Behavior tests** — minimal setup (focused, readable)
- **Mutation guard test** — full setup (catches mutations that only occur when fields have values)

**Requires:** Object must be Equatable.

---

## Equivalence Classes for Numeric Inputs

For any numeric parameter, consider three equivalence classes:
- **Positive** - the typical case
- **Zero** - boundary, often special behavior
- **Negative** - may be invalid, may have different behavior

Don't assume the code handles these correctly. Test all three.

---

## Adding New Principles

When user feedback reveals a gap in TCO quality:
1. Extract the general principle
2. Add it to this file with examples
3. Principles should be actionable and verifiable
