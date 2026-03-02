# Debugging Anti-Patterns

Patterns that feel productive but waste time. Recognize them, break out of them.

## The Big 7

### 1. Shotgun Debugging

**What it looks like:** Changing random things until the bug disappears.

**Why it fails:** Even if the bug stops, you don't know why. You may have introduced a new bug that won't surface until production.

**Fix:** Follow the 4-phase cycle. Reproduce, gather evidence, hypothesize, then fix. One change at a time.

---

### 2. Cargo Cult Fixing

**What it looks like:** Copying a fix from Stack Overflow or a past bug without understanding why it works.

**Why it fails:** The fix might address a different root cause. It might work now and break later. You can't explain it in code review.

**Fix:** Before applying any fix, state why it works. "This fixes the bug because [specific mechanism]." If you can't finish that sentence, you don't understand the fix yet.

---

### 3. Print Statement Avalanche

**What it looks like:** Adding 30 console.log statements and scanning output.

**Why it fails:** Too much noise drowns out the signal. You spend more time reading logs than understanding code.

**Fix:** Add 2-3 targeted log statements at decision points. Use them to confirm or refute a specific hypothesis. Remove them when done.

---

### 4. Blame-Driven Debugging

**What it looks like:** "It must be the library." "The framework has a bug." "The API changed."

**Why it fails:** External causes are real but rare. Starting with blame skips the most likely causes: your code, your config, your assumptions.

**Fix:** Assume your code is wrong first. Only blame externals after you've verified your code is correct AND the external behavior doesn't match its docs.

---

### 5. Tunnel Vision

**What it looks like:** Spending 2 hours in one file because you're sure the bug is there.

**Why it fails:** The bug is often somewhere else. The symptom shows in file A, but the cause is in file B, C, or the interaction between them.

**Fix:** After 15 minutes without progress in one location, step back. Trace the data flow from the beginning. The bug is where the data first goes wrong, not where the error finally surfaces.

---

### 6. The Infinite Loop

**What it looks like:** "Let me try one more thing..." repeated 10 times with the same approach.

**Why it fails:** If your approach hasn't worked after 3 attempts, the approach is wrong. More attempts won't help.

**Fix:** After 3 failed attempts with the same strategy, stop. Switch to ACH: list all hypotheses, build an evidence matrix, eliminate systematically.

---

### 7. Fix-First Debugging

**What it looks like:** Immediately writing a fix before understanding the bug.

**Why it fails:** You're fixing your assumption, not the actual bug. The "fix" often introduces new issues or masks the real problem.

**Fix:** No code changes until you can answer: "What exactly is happening, and why?" Phase 2 (Gather Evidence) comes before Phase 4 (Fix).

---

## Red Flags That You're In an Anti-Pattern

| Red Flag | You Might Be In |
|----------|----------------|
| "Let me just try..." | Shotgun Debugging |
| "This worked for someone else" | Cargo Cult Fixing |
| 10+ log statements added | Print Avalanche |
| "It's definitely not my code" | Blame-Driven |
| Same file open for 30+ minutes | Tunnel Vision |
| 4th attempt with same approach | Infinite Loop |
| Writing fix before reading code | Fix-First |

## Recovery Protocol

When you catch yourself in an anti-pattern:

1. **Stop.** Close the file you're staring at.
2. **Re-read the error.** The actual error message, from the top.
3. **State what you know.** Only facts, no theories.
4. **State what you don't know.** These are your investigation targets.
5. **Pick one unknown.** Investigate that. One at a time.

This resets your debugging loop and gets you back on the 4-phase cycle.
