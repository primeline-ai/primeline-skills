---
name: code-review
description: Use when reviewing pull requests, reviewing your own code before committing, or when asked to review code changes. Provides structured analysis with severity categorization, security focus, and optional worktree isolation for safe PR review.
---

# Code Review

Structured code review that catches what matters. Not style nitpicks - real bugs, security holes, and logic errors.

**Announce:** "Using code-review to analyze these changes."

## Quick Start (Self-Review)

Before committing your own code, check these 3 things:

1. **Does it break existing tests?** Run the full test suite. Not just your new tests.
2. **Does it handle the error case?** For every happy path, ask: "What if this fails?"
3. **Does it leak secrets or PII?** Search your diff for API keys, passwords, tokens, email addresses.

If all 3 pass, your code is ready for a proper review.

## The Full System

### Step 1: Understand the Context

Before reading a single line of code:

- What is this change supposed to do? (Read the PR description or commit message)
- What files are involved? (Get the scope)
- Are there unaddressed comments from previous reviewers?

```bash
# For PRs:
gh pr view <number> --json title,body,files
gh pr diff <number>
```

### Step 2: Set Up Safe Review (PRs Only)

For reviewing other people's PRs, use a git worktree for isolation:

```bash
# Create isolated workspace
git worktree add .worktrees/pr-<number> -b review/pr-<number>
cd .worktrees/pr-<number>

# Check out the PR
gh pr checkout <number>
```

**Why isolation?** The PR may contain untrusted code. Never run code from a PR you haven't reviewed. A worktree keeps your working directory clean.

### Step 3: Analyze the Changes

Read the diff file by file. For each file, check:

**Correctness:**
- Does the logic do what the description says?
- Are edge cases handled? (null, empty, zero, negative, overflow)
- Are error paths handled? (network failure, disk full, permission denied)
- Do types match? (especially at boundaries - API responses, user input)

**Security:**
- Input validation on user-provided data?
- SQL/NoSQL injection possible?
- XSS possible in rendered output?
- Auth checks on new endpoints?
- Secrets hardcoded anywhere?
- Unsafe deserialization?

**Quality:**
- Does the change follow existing patterns in the codebase?
- Are there obvious simplifications?
- Is there dead code or unused imports?
- Are variable names clear enough to understand without comments?

### Step 4: Categorize Findings

Every finding gets a severity:

| Severity | Meaning | Action |
|----------|---------|--------|
| **Critical** | Security vulnerability, data loss, crash in production | Must fix before merge |
| **High** | Bug, race condition, significant logic error | Should fix before merge |
| **Medium** | Code quality, missing edge case, maintainability | Should address, not blocking |
| **Low** | Style, naming suggestion, minor improvement | Optional |

**Each finding needs:**
- File and line reference (`src/auth.ts:45`)
- What the issue is (specific, not vague)
- Why it matters (impact)
- Suggested fix (actionable, not "please improve")

### Step 5: Write the Review

Structure your review:

```markdown
## Review: [PR Title or Change Description]

### Summary
[1-2 sentences: what this change does and overall assessment]

### Critical / High
[List issues that must or should be fixed]
- `file:line` - [Issue]. [Why it matters]. Fix: [suggestion].

### Medium
[Issues worth addressing]
- `file:line` - [Issue]. Fix: [suggestion].

### Low
[Optional improvements]
- `file:line` - [Suggestion].

### Positive Notes
[What's done well - this matters for morale and learning]

### Verdict
[Approve / Request Changes / Comment]
```

### Step 6: Clean Up (PRs Only)

```bash
# Return to main workspace
cd ../..
git worktree remove .worktrees/pr-<number>
git branch -D review/pr-<number>
```

## When to Use

- Reviewing a pull request (yours or someone else's)
- Before committing code to a shared branch
- After implementing a feature, before marking it done
- When you suspect code quality issues in a section of the codebase

## When NOT to Use

- Reviewing documentation-only changes (just read them)
- Reviewing auto-generated code (CI configs, lock files)
- When the user explicitly says "skip review"
- For exploratory or prototype code that will be rewritten

## Delegation Pattern

Large PRs (500+ lines) benefit from delegation:

```markdown
## 1. TASK
Review the diff of PR #[NUMBER] for bugs, security issues, and code quality.

## 2. EXPECTED OUTCOME
Categorized findings (Critical/High/Medium/Low) with file:line references and suggested fixes.

## 3. REQUIRED TOOLS
Read, Grep, Glob, Bash (gh pr view, gh pr diff only)

## 4. MUST DO
- Read every changed file
- Check for security vulnerabilities (OWASP top 10)
- Check for logic errors and unhandled edge cases
- Categorize findings by severity
- Include specific file:line references

## 5. MUST NOT DO
- Don't run any code from the PR
- Don't modify any files
- Don't approve or merge the PR
- Don't report style-only issues as High/Critical

## 6. CONTEXT
PR: [URL or number]
Description: [What the PR does]
Key files: [Most important files to focus on]
```

**Agent:** general-purpose | **Model:** Sonnet | **Effort:** High (12 turns)

## Security Focus Areas

These are the highest-value things to check. A review that catches one of these pays for itself:

| Area | What to Check |
|------|--------------|
| **Auth bypass** | New endpoints without auth middleware? Role checks missing? |
| **Injection** | User input reaching SQL, shell commands, or HTML without sanitization? |
| **Secrets** | API keys, tokens, passwords in code or config committed to repo? |
| **SSRF** | User-controlled URLs being fetched server-side? |
| **Path traversal** | User input in file paths without validation? |
| **Mass assignment** | User input spread into database objects without whitelisting fields? |

## Anti-Patterns

| Pattern | Problem | Fix |
|---------|---------|-----|
| "LGTM" with no comments | Rubber-stamp review, catches nothing | Spend at least 5 minutes per 100 lines |
| Style-only feedback | Misses real bugs while bikeshedding | Check security and correctness first, style last |
| "This could be better" | Vague, not actionable | Specific: what, where, why, how to fix |
| Reviewing 2000 lines at once | Attention drops after ~400 lines | Request smaller PRs or review in chunks |
| Skipping test files | Tests might assert wrong behavior | Review tests - they're the spec |

## With Starter System

If you have the [Starter System](https://github.com/primeline-ai/claude-code-starter-system) installed:
- Use `/remember` to log review patterns specific to your codebase
- "Always check for X in Y files" becomes institutional knowledge
- Use `/handoff` to pass review context across sessions

## With Kairn (Course)

When [Kairn](https://primeline.cc) is configured, code review gains:
- **Cross-project patterns** - "This auth pattern was flagged in 3 other projects"
- **Review history** - tracks which types of issues your codebase tends to have
- **Codebase-specific rules** - learns your project's conventions over time

Without Kairn: review quality depends on the reviewer's memory. Still structured, still valuable, just not adaptive.
