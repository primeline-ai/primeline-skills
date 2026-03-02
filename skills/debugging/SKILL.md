---
name: systematic-debugging
description: Use when encountering any bug, test failure, or unexpected behavior. Provides a structured 4-step cycle and ACH methodology for complex bugs. Replaces random guessing with evidence-based investigation.
---

# Systematic Debugging

Stop guessing. Start investigating. Every bug follows a pattern - find the pattern, find the fix.

**Announce:** "Using systematic-debugging to investigate this issue."

## Quick Start (3 Steps)

For straightforward bugs, this is often enough:

1. **Read the error.** Actually read it. File, line, message. Don't skip this.
2. **Reproduce it.** Run the failing command. See the exact output. No assumptions.
3. **Trace backward.** From the error line, follow the data. Where did the wrong value come from?

If these 3 steps solve it, you're done. If not, use the full system below.

## The Full System: 4-Phase Cycle

### Phase 1: Reproduce

**Goal:** See the bug happen with your own eyes.

- Run the exact command or action that triggers the bug
- Record the full error output (not a summary - the actual output)
- Note what you expected vs what you got
- Check: is the bug consistent or intermittent?

**If you can't reproduce it, you can't fix it.** Gather more context first.

### Phase 2: Gather Evidence

**Goal:** Collect facts, not theories.

Read the relevant code. Don't guess what it does - read it.

```
Evidence Checklist:
- [ ] Full error message and stack trace
- [ ] The source code at the error location
- [ ] Recent changes to related files (git log --oneline -10 -- <file>)
- [ ] Input data that triggers the bug
- [ ] What the code should do vs what it actually does
```

**Rule: Observe before editing.** If you haven't read the code, you don't understand the bug. If you don't understand the bug, your fix will create the next bug.

### Phase 3: Form Hypotheses

**Goal:** Generate 2-3 possible explanations. Then test them.

For each hypothesis:
1. State it clearly: "I think X fails because Y"
2. Define a test: "If this hypothesis is correct, then Z should be true"
3. Run the test
4. Record the result: confirmed, refuted, or inconclusive

**Don't stop at the first hypothesis that feels right.** The most obvious explanation is wrong about 40% of the time.

### Phase 4: Fix and Verify

**Goal:** Make the smallest change that fixes the bug. Verify it works.

1. Make the fix
2. Run the reproduction case - does it pass now?
3. Run the full test suite - did you break anything else?
4. Explain why the fix works (if you can't explain it, you didn't fix it)

**Done.** If the fix doesn't work, go back to Phase 2 with new evidence.

## ACH Method (Complex Bugs)

When a bug survives the 4-phase cycle, use Analysis of Competing Hypotheses:

### Step 1: List All Hypotheses

Write down every possible explanation, including ones that seem unlikely:

```
H1: The database query returns stale cached data
H2: The auth token expires between request and response
H3: A race condition in the async handler
H4: The input validation silently drops a required field
```

### Step 2: Build an Evidence Matrix

| Evidence | H1 (Cache) | H2 (Token) | H3 (Race) | H4 (Validation) |
|----------|-----------|-----------|----------|----------------|
| Bug is intermittent | Consistent | Consistent | **Supports** | Inconsistent |
| Only happens under load | Neutral | Neutral | **Supports** | Inconsistent |
| Error log shows timeout | Inconsistent | **Supports** | **Supports** | Inconsistent |
| Fresh deploy doesn't fix | **Supports** | Neutral | Neutral | **Supports** |

### Step 3: Eliminate

Remove hypotheses that are inconsistent with the evidence. The survivor is your best lead.

In the example above, H4 is inconsistent with two pieces of evidence - eliminate it first. H1 is inconsistent with one - it's less likely. H2 and H3 both survive - investigate the one that's easier to test first.

### When to Use ACH vs Quick Debug

| Situation | Method |
|-----------|--------|
| Clear error message, obvious location | Quick Start (3 steps) |
| Known area, unclear cause | 4-Phase Cycle |
| Multiple possible causes, intermittent | ACH Method |
| Bug persists after 2+ fix attempts | ACH Method |

## When to Use This Skill

- Test fails and you don't immediately know why
- An error message appears in logs or console
- Code behaves differently than expected
- A previously working feature breaks
- Performance degrades without obvious cause

## When NOT to Use

- You already know the exact fix (just fix it)
- It's a typo or syntax error (your linter already told you)
- The "bug" is actually a missing feature (use planning instead)

## Delegation Pattern

Complex bugs benefit from delegation. Score: Exploration (+3) + 3+ files (+2) = 5+. Delegate the investigation, keep the fix.

```markdown
## 1. TASK
Investigate why [SYMPTOM] occurs when [TRIGGER].

## 2. EXPECTED OUTCOME
Root cause analysis: error location (file:line), why it fails, suggested fix.

## 3. REQUIRED TOOLS
Read, Grep, Glob, Bash (read-only: test output, git log)

## 4. MUST DO
- Reproduce the error by running: [COMMAND]
- Read the source at the error location
- Check git log for recent changes
- Form at least 2 hypotheses and test each

## 5. MUST NOT DO
- Don't modify any files
- Don't run the application in production mode
- Don't access external services

## 6. CONTEXT
Error: [FULL_ERROR_MESSAGE]
Relevant files: [FILE_LIST]
Last working state: [COMMIT_OR_DATE]
```

**Agent:** debugger | **Model:** Sonnet | **Effort:** Medium (6 turns)

## Anti-Patterns

| Pattern | Problem | Fix |
|---------|---------|-----|
| "Let me just try this..." | Random changes without understanding | Phase 2 first - gather evidence |
| Fixing the symptom | Suppressing the error instead of the cause | Trace to the root, not the surface |
| Changing multiple things at once | Can't tell which change fixed it | One change, one test, one commit |
| Ignoring the stack trace | The answer is literally in the output | Read the error. All of it. |
| "It works on my machine" | Environment assumptions | Reproduce in the failing environment |
| Debugging in production | Risk of data loss or downtime | Reproduce locally first |

For the extended guide with recovery protocols, see `references/anti-patterns.md`.

## With Starter System

If you have the [Starter System](https://github.com/primeline-ai/claude-code-starter-system) installed:
- Use `/remember failure: [description]` to log bugs you've seen before
- Check `/remember` at the start of debugging - the bug might be a known pattern
- Use `/handoff` to pass debugging context when you run out of session time

## With Kairn (Course)

When [Kairn](https://primeline.cc) is configured, debugging gains:
- **Recall before investigating:** "Have I seen this error pattern before?" - automatic lookup across past sessions
- **Learn after resolution:** captures root cause + fix for future sessions automatically
- **Pattern recognition** across past bugs - "This looks like the auth timeout issue from 3 weeks ago"

Without Kairn: each debugging session starts fresh. The 4-phase cycle still works, you just can't learn from past sessions automatically.
