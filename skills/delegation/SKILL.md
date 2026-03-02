---
name: smart-delegation
description: Use when facing any non-trivial task to decide whether to handle it yourself or delegate to a subagent. Provides score-based routing, model selection, and structured prompt templates for effective delegation.
---

# Smart Delegation

Route tasks to the right agent at the right model tier. Stop wasting Opus tokens on file searches and Haiku tokens on architecture decisions.

**Announce:** "Using smart-delegation to route this task."

## Quick Start

Before starting any task, score it:

```
Score >= 3 → Delegate to subagent
Score < 3  → Handle it yourself
```

**Scoring (add up what applies):**

| Factor | Points |
|--------|--------|
| Touches 3+ files | +2 |
| Bulk/repetitive operation | +2 |
| Research or learning task | +2 |
| Code review | +2 |
| Exploration or search | +3 |
| Independent (no shared state) | +2 |

**Subtract:**

| Factor | Points |
|--------|--------|
| Contains: production, deploy, secret, password | -10 |
| User said "show me" or "explain" | -5 |

Score >= 3? Delegate. Move on.

## The Full System

### Step 1: Score the Task

Run the scoring table above. If the score is negative or zero, do it yourself.

### Step 2: Pick the Model

Match complexity to model. Don't overthink this.

| Complexity | Model | Use For |
|------------|-------|---------|
| 1-2 | Haiku | File search, pattern finding, simple lookups |
| 3-6 | Sonnet | Bug fixes, research, planning, code review |
| 7+ | Don't delegate | Deep architecture, multi-system reasoning |

**Why not delegate 7+?** Complex tasks need your full context window. Subagents start fresh with zero context - they'll miss critical connections.

### Step 3: Pick the Agent Type

| Task | Agent Type | Why |
|------|-----------|-----|
| Find files, explore code | `Explore` | Built for fast codebase search |
| Debug failing tests | `debugger` | Structured hypothesis testing |
| Plan implementation | `Plan` | Architecture-first thinking |
| Everything else | `general-purpose` | Full tool access |

### Step 4: Write the Prompt

Every delegation prompt needs 6 sections. No exceptions.

```markdown
## 1. TASK
What to do. One sentence. Be specific.

## 2. EXPECTED OUTCOME
What the agent should return. Format, structure, length.

## 3. REQUIRED TOOLS
Which tools the agent should use. Be explicit.

## 4. MUST DO
Non-negotiable requirements. List each one.

## 5. MUST NOT DO
Guardrails. What the agent must avoid.

## 6. CONTEXT
File paths, patterns, constraints the agent needs to know.
```

**Why 6 sections?** Subagents have zero context about your project. The prompt IS their entire world. Vague prompts produce vague results.

### Step 5: Set Effort Level

| Effort | max_turns | When |
|--------|-----------|------|
| Low | 3 | File search, quick lookups |
| Medium | 6 | Standard tasks (default) |
| High | 12 | Thorough review, edge cases |
| Max | 25 | Security audit, full architecture |

### Step 6: Verify the Result

After every delegation, check:
- [ ] Does the result match EXPECTED OUTCOME?
- [ ] Were all MUST DO items addressed?
- [ ] Were all MUST NOT DO items respected?

**No verification = task not complete.** Trusting without checking is the #1 delegation failure.

## When to Use

- Task scores >= 3 on the scoring table
- You're running low on context window (delegation saves ~90% of tokens vs doing it yourself)
- Multiple independent tasks can run in parallel
- The task is well-defined enough to describe in 6 sections

## When NOT to Use

- Task involves secrets, credentials, or production systems
- User explicitly wants to see the process ("show me", "walk me through")
- Task requires deep cross-file reasoning with shared state
- You can't clearly define the expected outcome (if you can't describe it, don't delegate it)

## Examples

### Example 1: Delegating a Code Search

**Score:** Exploration (+3) + Independent (+2) = 5. Delegate.

```markdown
## 1. TASK
Find all API endpoint definitions in the codebase.

## 2. EXPECTED OUTCOME
List of endpoints with: HTTP method, path, handler file, line number.

## 3. REQUIRED TOOLS
Grep, Glob, Read

## 4. MUST DO
- Search for route definitions (app.get, app.post, router.*, @app.route, etc.)
- Include both REST and GraphQL endpoints
- Report file path and line number for each

## 5. MUST NOT DO
- Don't modify any files
- Don't execute any code

## 6. CONTEXT
Project uses Next.js with API routes in src/app/api/
```

**Agent:** Explore | **Model:** Haiku | **Effort:** Low (3 turns)

### Example 2: Delegating a Bug Investigation

**Score:** Exploration (+3) + 3+ files (+2) + Independent (+2) = 7. Delegate.

```markdown
## 1. TASK
Investigate why the email signup form returns 500 on submit.

## 2. EXPECTED OUTCOME
Root cause analysis with: error location, why it fails, suggested fix.

## 3. REQUIRED TOOLS
Read, Grep, Glob, Bash (for checking logs only)

## 4. MUST DO
- Read the form component and its submit handler
- Read the API route that handles the submission
- Check for recent changes to related files (git log)
- Identify the exact line where the error originates

## 5. MUST NOT DO
- Don't modify any files
- Don't run the application
- Don't access any external services

## 6. CONTEXT
Form component: src/components/email-capture.tsx
API route: src/app/api/subscribe/route.ts
Error started after last deploy (2 days ago)
```

**Agent:** debugger | **Model:** Sonnet | **Effort:** Medium (6 turns)

### Example 3: Parallel Delegation

Three independent tasks? Launch them simultaneously:

```
Task A: Search for unused imports     → Explore, Haiku, Low
Task B: Review auth middleware        → general-purpose, Sonnet, High
Task C: Find test coverage gaps       → general-purpose, Sonnet, Medium
```

All three run in parallel. You get results from all three before deciding next steps.

## Anti-Patterns

| Pattern | Problem | Fix |
|---------|---------|-----|
| "Just look into this" | No expected outcome | Define what "done" looks like |
| Delegating with full context | Agent gets 200K tokens of noise | Give only relevant context |
| Not verifying results | Agent may hallucinate or miss things | Always run Step 6 |
| Delegating 7+ complexity | Agent lacks your mental model | Do it yourself |
| Sequential when parallel works | Wastes time | Launch independent tasks together |

## Security Guardrails

**Never delegate tasks involving:**
- API keys, tokens, passwords, secrets
- Production database operations
- Deployment or release processes
- User data access or PII handling
- Payment or financial operations

These require your direct attention and the user's explicit approval.

## With Starter System

If you have the [Starter System](https://github.com/primeline-ai/claude-code-starter-system) installed:
- Use `/remember` to log delegation patterns that work well
- Check `/context-stats` before delegating - high context usage makes delegation more valuable
- Use `/handoff` to pass delegation insights to the next session

## With Kairn (Course)

When [Kairn](https://primeline.cc) is configured, delegation gains:
- **Adaptive scoring** - learns which tasks delegate well and which don't from your history
- **Outcome tracking** - records delegation success/failure to refine routing over time
- **Cross-session patterns** - "Last 5 times you delegated auth reviews to Sonnet, 4 succeeded"

Without Kairn: scoring is manual and each session starts fresh. Still valuable, just not adaptive.
