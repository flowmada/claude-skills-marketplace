# When to Use Formal Test Artifacts

Examples to help decide: formal analysis or narrative?

## Skip Formal Artifacts

### Example: Simple State Machine

```swift
func state(now: Date) -> State {
    if intoFridgeAt != nil { return .fermenting }
    guard sessionStartedAt != nil else { return .waiting }
    guard let mixDone = mixingCompletedAt else { return .mixing }
    let restElapsed = now.timeIntervalSince(mixDone)
    return restElapsed < counterRestDuration ? .resting : .readyForFridge
}
```

**Why skip:** Branches are obvious. Each guard/if maps to one state. No hidden interactions. "Analysis" would just restate the code.

**Narrative instead:**
> Test each state transition: waiting (not started), mixing (started, not rested), resting (rest in progress), readyForFridge (rest complete), fermenting (in fridge). One boundary: restElapsed vs counterRestDuration.

### Example: Simple Boolean Check

```swift
var isComplete: Bool { intoFridgeAt != nil }
```

**Why skip:** Two cases. Obvious.

### Example: Straightforward Formatting

```swift
var displayName: String { "\(firstName) \(lastName)" }
```

**Why skip:** No edge cases worth formal analysis. Maybe test empty strings if relevant.

---

## Use Formal Artifacts

### Example: Time/Duration Formatting

```swift
func waitingCountdown(now: Date) -> String {
    let interval = scheduledStart.timeIntervalSince(now)
    let totalMinutes = Int(abs(interval) / 60)
    let hours = totalMinutes / 60
    let minutes = totalMinutes % 60
    
    if abs(interval) < 60 { return "Now" }
    else if interval > 0 {
        return hours > 0 ? "in \(hours)h \(minutes)m" : "in \(minutes)m"
    } else {
        return hours > 0 ? "\(hours)h ago" : "\(minutes)m ago"
    }
}
```

**Why use artifacts:**
- Multiple boundaries (60 seconds, 0, hour threshold)
- Different output formats based on conditions
- Easy to miss edge cases

**Equivalence classes:**
- interval: {within 60s of now, future with hours, future minutes-only, past with hours, past minutes-only}

**Boundaries:**
- 59s, 60s, 61s (Now threshold)
- 59m, 60m, 61m (hour threshold)
- -1s, 0s, +1s (past/future)

### Example: Validation with Multiple Rules

```swift
func validate(password: String) -> [ValidationError] {
    var errors: [ValidationError] = []
    if password.count < 8 { errors.append(.tooShort) }
    if password.count > 64 { errors.append(.tooLong) }
    if !password.containsUppercase { errors.append(.needsUppercase) }
    if !password.containsNumber { errors.append(.needsNumber) }
    return errors
}
```

**Why use artifacts:**
- Multiple independent conditions
- Combinations matter (can have multiple errors)
- Boundaries on length

**Test matrix helps:** Shows which combinations to test.

### Example: Interacting Dimensions

```swift
func calculatePrice(quantity: Int, memberLevel: MemberLevel, hasPromo: Bool) -> Decimal
```

**Why use artifacts:**
- Three inputs interact
- Business rules may have non-obvious combinations
- Matrix reveals gaps

---

## Decision Checklist

Ask yourself:

- [ ] Are the cases obvious from reading the code?
- [ ] Would analysis just restate the structure?
- [ ] Are there subtle boundaries?
- [ ] Do multiple inputs interact?
- [ ] Are there business rules with non-obvious combinations?

**Mostly "yes" to first two, "no" to rest → Skip formal artifacts**

**Any "yes" to last three → Use formal artifacts**

---

## When Unsure

Start with narrative. If you find yourself listing many cases or worrying about gaps, switch to formal artifacts. The checkpoint exists to catch this.
