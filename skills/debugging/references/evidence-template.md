# Evidence Collection Template

Use this template when investigating bugs. Copy it, fill it in as you go. Structured evidence prevents circular debugging.

## Bug Report

```markdown
## Bug: [One-line description]

**Reported:** [Date]
**Severity:** [Critical / High / Medium / Low]
**Status:** [Investigating / Root cause found / Fixed / Won't fix]

---

### 1. Symptoms

**What happens:**
[Exact behavior observed]

**What should happen:**
[Expected behavior]

**Reproduction steps:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Reproduction rate:** [Always / ~70% / Intermittent / Once]

**Environment:**
- OS: [e.g., macOS 14.5]
- Node: [e.g., 20.11.0]
- Browser: [if applicable]
- Last known working: [commit hash or date]

---

### 2. Error Output

```
[Paste FULL error message and stack trace here]
[Do not summarize - paste the actual output]
```

---

### 3. Evidence Collected

| # | Evidence | Source | Timestamp |
|---|----------|--------|-----------|
| 1 | [What you found] | [file:line or command] | [When] |
| 2 | [What you found] | [file:line or command] | [When] |
| 3 | [What you found] | [file:line or command] | [When] |

---

### 4. Hypotheses

| # | Hypothesis | Test | Result |
|---|-----------|------|--------|
| H1 | [Statement] | [How to test] | [Confirmed / Refuted / Inconclusive] |
| H2 | [Statement] | [How to test] | [Confirmed / Refuted / Inconclusive] |
| H3 | [Statement] | [How to test] | [Confirmed / Refuted / Inconclusive] |

---

### 5. Root Cause

**What:** [The actual cause]
**Where:** [file:line]
**Why:** [Why this caused the symptoms]

---

### 6. Fix

**Change:** [What was changed]
**File(s):** [Which files were modified]
**Verification:** [How it was verified - test name, manual check, etc.]

---

### 7. Lessons

**What to watch for next time:**
[Pattern to recognize in future bugs]
```

## ACH Evidence Matrix Template

For complex bugs with multiple competing hypotheses:

```markdown
## ACH Matrix: [Bug Description]

### Hypotheses

- **H1:** [Description]
- **H2:** [Description]
- **H3:** [Description]
- **H4:** [Description]

### Evidence Matrix

| Evidence | H1 | H2 | H3 | H4 |
|----------|----|----|----|----|
| [Evidence 1] | [S/I/N] | [S/I/N] | [S/I/N] | [S/I/N] |
| [Evidence 2] | [S/I/N] | [S/I/N] | [S/I/N] | [S/I/N] |
| [Evidence 3] | [S/I/N] | [S/I/N] | [S/I/N] | [S/I/N] |

**Legend:** S = Supports, I = Inconsistent, N = Neutral

### Elimination

- [x] **H4 eliminated:** Inconsistent with Evidence 1 and 3
- [x] **H1 eliminated:** Inconsistent with Evidence 2
- [ ] **H2 and H3 survive** - investigate H2 first (easier to test)

### Investigation Order

1. Test H2 by: [specific action]
2. If H2 refuted, test H3 by: [specific action]
```

## Quick Evidence Checklist

Before forming any hypothesis, collect these:

```
- [ ] Full error message (not a summary)
- [ ] Stack trace (complete, not truncated)
- [ ] The source code at error location (read it, don't assume)
- [ ] git log for recent changes to the file
- [ ] Input that triggers the bug
- [ ] What the code path should do (trace it)
- [ ] Environment details (versions, config)
```

If any of these are missing, gather them before theorizing. Evidence first, theories second.
