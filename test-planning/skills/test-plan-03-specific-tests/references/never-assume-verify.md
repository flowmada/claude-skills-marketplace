# Never Assume - Always Verify

## The Problem

When writing test plans, it's tempting to work from memory or domain knowledge:
- "I remember this uses `poolishIntoFridgeAt`..."
- "This probably calls the validation method..."
- "The ferment calculation should work like..."

**This is lazy and dangerous.** Your context is constantly changing. What you "remember" may be wrong, outdated, or conflated with similar code elsewhere.

## The Rule

**Never assume. Always verify.**

The code is on disk. You can read it. There is zero excuse for guessing when verification takes seconds.

Before writing Analysis or TCO:
1. **Read the code under test** - the actual file, not your memory of it
2. **Identify exact property names** - not what you think they should be
3. **Trace the data flow** - what does the function actually call?
4. **Verify dependencies** - what must be set up for this code to run?

## Why This Matters

Wrong assumptions cascade into:
- **Tests that don't compile** - using properties that don't exist
- **Tests that pass incorrectly** - testing the wrong thing entirely
- **Wasted time** - discovering the mistake late in implementation
- **Eroded trust** - the user can't rely on your TCO accuracy

## Examples

**Bad (assuming):**
> "The validation uses `poolishIntoFridgeAt` to calculate ferment elapsed..."

You assumed this based on domain knowledge. But the actual code calls `fermentProgress(at:)` which uses `scheduledFermentStart`. These are different properties.

**Good (verifying):**
> "Let me read the validation code first..."
> [Reads ScheduleState+DoughValidation.swift]
> "The function calls `fermentProgress(at: now)`. Let me check what that uses..."
> [Reads ScheduleState+FermentProgress.swift]
> "It uses `scheduledFermentStart`. The Arrange must set this property."

## Checklist

Before presenting Analysis:
- [ ] Have I read the code under test in this session?
- [ ] Do I know the exact property/method names from reading, not memory?
- [ ] Have I traced what the function actually calls?

Before presenting TCO:
- [ ] Does every Arrange property exist in the code I read?
- [ ] Am I using the exact names from the code, not synonyms?
- [ ] If uncertain about anything, have I re-read the code?

## The Standard

**100% certainty from verification, not confidence from assumption.**

If you find yourself thinking "I'm pretty sure it's..." - stop and read the code.
