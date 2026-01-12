# Identifying Test Cases

Techniques for systematically identifying which test cases to include when designing a TCO for a function or feature.

## Equivalence Partitioning

Divide inputs into classes where all values in a class should produce the same behavior. Include one test case per class.

### Example: Age Validation

```
Equivalence classes for age:
1. Negative → invalid
2. 0-17 → minor
3. 18-64 → adult
4. 65+ → senior

→ 4 test cases, one representative from each class
```

### Identifying Equivalence Classes

Look for:
- Ranges (min/max values)
- Categories (enum cases, type variants)
- States (empty, single, multiple)
- Conditions in code (if/else branches, switch cases)

## Boundary Testing

Test at the edges of equivalence classes where bugs commonly hide.

### Boundary Points

For a range like `18 <= x <= 64`:

| Point | Value | Description |
|-------|-------|-------------|
| Below min | 17 | Just outside lower bound |
| At min | 18 | Lower bound |
| Above min | 19 | Just inside lower bound |
| Nominal | 40 | Middle of range |
| Below max | 63 | Just inside upper bound |
| At max | 64 | Upper bound |
| Above max | 65 | Just outside upper bound |

### Example: Timeout Threshold

```
Request times out after 30 seconds

Test cases:
- 29 seconds → success (just inside)
- 30 seconds → success (at boundary)
- 31 seconds → timeout (just outside)
```

## Combining Both Techniques

1. **Identify equivalence classes** from requirements/code
2. **Pick boundaries** between classes
3. **Test boundaries** + one representative from each class interior

### Example: Complete TCO

```
Requirement: Password 8-20 chars

Equivalence classes:
- Too short (0-7)
- Valid (8-20)
- Too long (21+)

Test cases:
| ID | Scenario | Input | Expected |
|----|----------|-------|----------|
| PW-1 | Empty password | "" | too short |
| PW-2 | Below min boundary | 7 chars | too short |
| PW-3 | At min boundary | 8 chars | valid |
| PW-4 | Valid interior | 12 chars | valid |
| PW-5 | At max boundary | 20 chars | valid |
| PW-6 | Above max boundary | 21 chars | too long |
```

## Common Boundary Bugs

| Bug Pattern | Test For |
|-------------|----------|
| Off-by-one | `< vs <=`, `> vs >=` |
| Fence post | First/last element in collection |
| Empty state | Zero items, empty string, nil |
| Overflow | Max int, max date, max array size |
| Type limits | Int.max, Double.infinity, Date.distantFuture |

## Quick Checklist

When designing a TCO, ask:
- [ ] What are the equivalence classes?
- [ ] What are the boundaries between them?
- [ ] Have I included both sides of each boundary?
- [ ] Have I included the exact boundary value?
- [ ] What about empty/zero/nil cases?
- [ ] What error conditions can occur?
