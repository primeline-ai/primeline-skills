# Model Selection Matrix

Choosing the right model for delegation is about matching cognitive load to capability. Overshoot wastes money and time. Undershoot wastes the task.

## The Matrix

| Complexity | Model | Cost | Speed | Context | Best For |
|------------|-------|------|-------|---------|----------|
| 1-2 | **Haiku** | Lowest | Fastest | 200K | File search, grep patterns, simple lookups, list generation |
| 3-4 | **Sonnet** | Medium | Fast | 200K | Bug fixes, code review, research, standard implementations |
| 5-6 | **Sonnet** | Medium | Fast | 200K | Multi-file analysis, planning, complex review |
| 7+ | **Don't delegate** | - | - | - | Deep architecture, cross-system reasoning, novel design |

## Decision Rules

### Use Haiku When:
- The task is "find X" or "list Y"
- No reasoning required beyond pattern matching
- Result is a simple list or yes/no
- Task finishes in 1-3 turns

**Examples:**
- "Find all files importing AuthContext"
- "List all TODO comments in src/"
- "Check if package X is in package.json"

### Use Sonnet When:
- The task requires understanding code, not just finding it
- Multiple files need to be read and connected
- The agent needs to form and test hypotheses
- Output requires judgment (review, analysis, recommendation)

**Examples:**
- "Investigate why test X fails after the last commit"
- "Review this PR for security issues"
- "Research the best approach for adding WebSocket support"
- "Analyze the authentication flow for vulnerabilities"

### Don't Delegate When:
- You need to maintain a mental model across many files
- The task requires creative architectural decisions
- Prior conversation context is essential to the task
- The task is ambiguous enough that you'd need to ask clarifying questions
- The result will directly affect production systems

**Examples:**
- "Redesign the database schema for the new feature"
- "Decide between microservices and monolith for this project"
- "Implement the core business logic that ties 8 modules together"

## Effort Levels (max_turns)

| Level | Turns | Typical Duration | Use For |
|-------|-------|-----------------|---------|
| **Low** | 3 | 10-20 seconds | File search, quick lookups, simple checks |
| **Medium** | 6 | 30-60 seconds | Standard investigation, review, research |
| **High** | 12 | 1-3 minutes | Thorough review, edge case analysis, deep investigation |
| **Max** | 25 | 3-8 minutes | Security audit, full codebase analysis, exhaustive search |

**Default to Medium.** Only go higher when the task explicitly needs thoroughness (security, production code review). Only go lower when you just need a file found.

## Cost-Aware Patterns

### Parallel Haiku > Sequential Sonnet
If you have 5 independent search tasks, launch 5 Haiku agents in parallel instead of 1 Sonnet agent doing them sequentially. Faster and cheaper.

### Sonnet for Investigation, Haiku for Verification
Use Sonnet to find the root cause. Then use Haiku to verify the fix exists in the right files.

### Escalation Pattern
Start with the cheapest viable option. If the result is insufficient, escalate:
1. Try Haiku first (if task seems simple)
2. If result is shallow or wrong, retry with Sonnet
3. If still insufficient, do it yourself

This is cheaper than always starting with Sonnet, because most delegation tasks (searches, lookups) are genuinely simple.

## Common Mistakes

| Mistake | Why It's Wrong | Fix |
|---------|---------------|-----|
| Using Opus for delegation | Subagents don't benefit from 128K output - they return a summary | Use Sonnet, save Opus for your own reasoning |
| Haiku for code review | Haiku misses subtle bugs and security issues | Use Sonnet with High effort |
| Max effort for everything | Wastes time on tasks that finish in 2 turns | Default to Medium, escalate if needed |
| Not setting max_turns | Agent runs indefinitely, burning tokens on loops | Always set effort level |
