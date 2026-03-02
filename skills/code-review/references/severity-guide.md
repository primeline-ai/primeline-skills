# Severity Classification Guide

How to assign severity to review findings. When in doubt, go one level higher - it's easier to downgrade than to miss a critical bug.

## The Four Levels

### Critical - Must Fix Before Merge

The change will cause harm in production if merged as-is.

**Examples:**
- SQL injection via unsanitized user input
- Authentication bypass (missing auth check on endpoint)
- Hardcoded API key or password in source code
- Data loss scenario (delete without confirmation, no backup)
- Crash on common input (null pointer on required field)
- Breaking change to public API without migration path

**Template:**
```
CRITICAL: `src/api/users.ts:34` - SQL injection via string concatenation
User input flows directly into SQL query without parameterization.
Impact: Attacker can read/modify/delete any database record.
Fix: Use parameterized query: `db.query('SELECT * FROM users WHERE id = $1', [userId])`
```

### High - Should Fix Before Merge

The change has a real bug or significant issue that will cause problems.

**Examples:**
- Logic error that produces wrong results for edge cases
- Race condition in concurrent code
- Missing error handling that will crash on failure
- Off-by-one error in loop or pagination
- Memory leak (event listener never removed, growing array)
- Broken error recovery (catch block swallows error silently)

**Template:**
```
HIGH: `src/utils/paginate.ts:22` - Off-by-one in page calculation
`Math.ceil(total / pageSize) - 1` returns 0 pages when total equals pageSize.
Impact: Last page of results is inaccessible.
Fix: Remove the `- 1`: `Math.ceil(total / pageSize)`
```

### Medium - Should Address, Not Blocking

The change works but has quality issues that will cause problems later.

**Examples:**
- Missing input validation (works now, will break on bad input)
- Duplicated logic that should be extracted
- Unclear variable names that make code hard to maintain
- Missing test for an important code path
- Inconsistency with existing codebase patterns
- Error message that doesn't help the user fix the problem

**Template:**
```
MEDIUM: `src/components/form.tsx:67` - No validation on email field
The email field accepts any string. Invalid emails will fail silently at the API level.
Fix: Add basic format check before submission: `/^[^@]+@[^@]+\.[^@]+$/`
```

### Low - Optional Improvement

Suggestions that improve the code but don't affect correctness.

**Examples:**
- More descriptive variable name
- Simpler way to write the same logic
- Unused import that should be removed
- Comment that could be clearer
- Test that could cover an additional edge case

**Template:**
```
LOW: `src/services/auth.ts:12` - `data` could be more descriptive
Rename to `userCredentials` to clarify what this object contains.
```

## Decision Tree

```
Does this issue...
├── ...allow unauthorized access or data theft?
│   └── CRITICAL
├── ...cause data loss or corruption?
│   └── CRITICAL
├── ...crash the application on common input?
│   └── CRITICAL
├── ...produce wrong results?
│   ├── For common cases? → HIGH
│   └── For rare edge cases? → MEDIUM
├── ...fail silently (swallow errors)?
│   ├── In critical path (auth, payment, data)? → HIGH
│   └── In non-critical path? → MEDIUM
├── ...make the code harder to maintain?
│   └── MEDIUM
├── ...work correctly but could be cleaner?
│   └── LOW
└── ...only affect style/formatting?
    └── LOW (or don't report it)
```

## Context Matters

The same issue can be different severities depending on context:

| Issue | In Auth Module | In Admin Dashboard | In Dev Tool |
|-------|---------------|-------------------|-------------|
| Missing input validation | Critical | High | Medium |
| Unclear error message | High | Medium | Low |
| No rate limiting | Critical | High | Low |
| Unused variable | Low | Low | Low |

## Confidence Levels

If you're not sure about a finding, say so:

```
HIGH (confidence: medium): `src/api/webhook.ts:89` - Possible race condition
If two webhooks arrive within 50ms, both may process the same event.
I'm not 100% certain the mutex at line 45 covers this path - worth verifying.
Fix: Add lock check before processing.
```

Being honest about uncertainty is more valuable than false confidence.
