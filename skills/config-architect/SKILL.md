---
name: config-architect
description: Use when creating, restructuring, or scaling a CLAUDE.md file. Provides tiered loading patterns, organization templates, and token measurement guidance. Prevents the "unmanageable blob" problem.
---

# Config Architect

Your CLAUDE.md is how Claude Code understands your project. A good one makes everything faster. A bad one wastes tokens on every single message.

**Announce:** "Using config-architect to structure this configuration."

## Quick Start

If your CLAUDE.md doesn't exist yet, copy the starter template from `references/starter-template.md` and fill in the blanks. 5 minutes to a working config.

If your CLAUDE.md exists but feels messy, jump to "Migration Path" below.

## The Problem

CLAUDE.md files grow. What starts as 10 helpful lines becomes 200 lines of everything you've ever told Claude. The result:

- Every message costs more tokens (CLAUDE.md is loaded on every interaction)
- Important rules get buried in noise
- Contradictory instructions accumulate
- You can't find what you need

## The Solution: Tiered Loading

Not everything needs to be loaded every time. Split your config into tiers:

### Tier 1: Always Loaded (~500 tokens)

Your main `CLAUDE.md` file. Contains only what Claude needs on EVERY interaction.

```markdown
# Project Name

## Quick Reference
- Stack: Next.js 15 + TypeScript + Tailwind
- Test: npm test
- Build: npm run build
- Lint: npm run lint

## Core Rules
- Use TypeScript strict mode
- Components in src/components/
- API routes in src/app/api/
- Tests next to source files: component.test.ts

## Key Patterns
- Server components by default, "use client" only when needed
- Zod for all input validation
- Error boundaries around async components
```

**What belongs here:** Tech stack, file locations, build commands, 3-5 non-obvious rules that apply to every task.

**What doesn't belong:** Feature-specific instructions, one-time notes, long explanations.

### Tier 2: Loaded on Match (~300 tokens each)

Rules files in `.claude/rules/` that Claude loads when relevant keywords appear.

```
.claude/rules/
  testing.md       # Loaded when task involves tests
  api-design.md    # Loaded when task involves API routes
  database.md      # Loaded when task involves data models
  deployment.md    # Loaded when task involves deploy/CI
```

Each file: 10-30 lines covering one topic. Claude loads them automatically when the conversation matches.

**What belongs here:** Domain-specific instructions, detailed conventions for specific areas, patterns that only matter sometimes.

### Tier 3: Loaded on Demand (0 tokens default)

Knowledge files referenced explicitly when needed. Not in `.claude/rules/`.

```
docs/
  architecture.md   # Referenced when Claude needs the big picture
  api-reference.md  # Referenced when building new endpoints
  style-guide.md    # Referenced when design questions arise
```

**What belongs here:** Long reference documents, architectural decisions, style guides, onboarding docs. Things Claude should read when asked, not on every message.

## When to Use Each Tier

| Content | Tier | Why |
|---------|------|-----|
| Build command | 1 | Needed constantly |
| "Use Zod for validation" | 1 | Applies to every task |
| "Test API routes with supertest" | 2 | Only when writing API tests |
| "Database migration naming convention" | 2 | Only when touching DB |
| Full API documentation | 3 | Reference only |
| Architecture decision records | 3 | Background context |

## Migration Path: Flat to Tiered

If your CLAUDE.md is already a blob, follow these steps:

### Step 1: Measure Current Size

```bash
wc -c CLAUDE.md
```

| Size | Status |
|------|--------|
| < 2KB | Fine as-is. Don't over-engineer. |
| 2-5KB | Consider splitting the largest sections into Tier 2 |
| 5-10KB | Split into tiers. You're wasting tokens. |
| 10KB+ | Urgent. Every message pays for content Claude doesn't need. |

### Step 2: Categorize Every Section

Read your CLAUDE.md line by line. For each section, ask:

- "Does Claude need this on EVERY task?" -> Keep in Tier 1
- "Does Claude need this for SOME tasks?" -> Move to Tier 2 (`.claude/rules/`)
- "Is this reference material?" -> Move to Tier 3 (`docs/`)
- "Is this outdated or redundant?" -> Delete it

### Step 3: Extract to Tiers

Move sections to their new homes. Keep Tier 1 under 500 tokens (~50 lines of concise instructions).

### Step 4: Verify

After restructuring, test with a representative task:
- Does Claude still follow the rules?
- Does it pick up Tier 2 rules when relevant?
- Is the token count visibly lower?

## Token Measurement

Rough estimates for planning:

| Content | Tokens (approx) |
|---------|-----------------|
| 1 line of instruction | ~10-15 tokens |
| 10-line section | ~100-150 tokens |
| Typical Tier 1 CLAUDE.md | ~300-500 tokens |
| Typical Tier 2 rule file | ~200-400 tokens |
| Every token in Tier 1 | Loaded on EVERY message |

**The math:** A 10KB CLAUDE.md is ~2,500 tokens. Over 100 messages in a session, that's 250,000 tokens spent just on config. A 2KB Tier 1 + selective Tier 2 loading cuts this by 60-80%.

## When to Use This Skill

- Starting a new project (set up config right from the start)
- CLAUDE.md is over 5KB and growing
- You notice Claude ignoring rules (too much noise)
- Adding a new team member who needs to set up their config
- Migrating from a flat CLAUDE.md to tiered structure

## When NOT to Use

- CLAUDE.md is under 2KB and working fine (don't fix what isn't broken)
- You're just adding one rule (just add it, no restructuring needed)
- The project is a one-off script (a 5-line CLAUDE.md is perfect)

## Examples

### Example 1: New Project Setup

You're starting a Next.js project. Use the starter template from `references/starter-template.md`:

1. Copy the template to `CLAUDE.md`
2. Fill in: stack, build command, test command, lint command
3. Add 3-5 project-specific rules
4. Done. Under 500 tokens, covers 90% of interactions.

### Example 2: Splitting a Bloated CLAUDE.md

Your CLAUDE.md is 8KB with sections for testing, API design, database patterns, deployment, and code style.

**Before:** Everything in one file. 2,000 tokens loaded on every message.

**After:**
```
CLAUDE.md (Tier 1, ~400 tokens)
  - Stack, build commands, 5 core rules

.claude/rules/testing.md (Tier 2, ~250 tokens)
  - Test framework, mocking patterns, coverage rules

.claude/rules/api-design.md (Tier 2, ~300 tokens)
  - REST conventions, error format, auth patterns

docs/database-patterns.md (Tier 3, 0 tokens default)
  - Migration naming, indexing strategy, query patterns
```

Result: 400 tokens per message instead of 2,000. Tier 2 loads only when relevant.

## Security Note

**Never put secrets in CLAUDE.md or rule files.** These are committed to version control. Use environment variables or `.env` files (which should be in `.gitignore`) for API keys, tokens, and credentials.

If Claude needs to know about a secret's existence (e.g., "this API requires an API key"), reference the environment variable name, not the value: `STRIPE_API_KEY` is set in `.env`.

## Anti-Patterns

| Pattern | Problem | Fix |
|---------|---------|-----|
| Everything in CLAUDE.md | Token waste on every message | Tier 2 for domain-specific rules |
| Rules without examples | Claude interprets vaguely | Add one concrete example per rule |
| Contradictory rules | Claude picks one randomly | Audit for conflicts quarterly |
| "Don't do X" without "do Y instead" | Claude knows what to avoid but not what to do | Always pair negatives with positives |
| Copy-pasting docs into CLAUDE.md | Bloats Tier 1 with reference material | Link to docs, don't inline them |
| No CLAUDE.md at all | Claude guesses your conventions | 5 minutes with the starter template |

## With Starter System

If you have the [Starter System](https://github.com/primeline-ai/claude-code-starter-system) installed:
- Your Starter System IS Tier 1 for session management
- Layer project-specific CLAUDE.md on top
- Use `/context-stats` to see actual token usage of your config
- Use `/remember` to note config patterns that work

## With Kairn (Course)

When [Kairn](https://primeline.cc) is configured, configuration gains:
- **Auto-detection** of rules Claude needs based on task patterns
- **Dynamic loading** that goes beyond keyword matching
- **Rule effectiveness tracking** - which rules actually change Claude's behavior

Without Kairn: tiered loading still works, just based on static keyword matching. You manually decide what goes where.
