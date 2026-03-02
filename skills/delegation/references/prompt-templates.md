# Delegation Prompt Templates

Ready-to-use templates for common delegation scenarios. Copy, fill in the blanks, delegate.

## Template 1: Code Exploration

```markdown
## 1. TASK
Find all [PATTERN] in the codebase.

## 2. EXPECTED OUTCOME
List with: file path, line number, surrounding context (3 lines).

## 3. REQUIRED TOOLS
Grep, Glob, Read

## 4. MUST DO
- Search in [DIRECTORIES]
- Include [FILE_TYPES]
- Report exact line numbers

## 5. MUST NOT DO
- Don't modify any files
- Don't execute any code

## 6. CONTEXT
Project structure: [BRIEF_DESCRIPTION]
Focus areas: [KEY_DIRECTORIES]
```

**Agent:** Explore | **Model:** Haiku | **Effort:** Low

---

## Template 2: Bug Investigation

```markdown
## 1. TASK
Investigate why [SYMPTOM] occurs when [TRIGGER].

## 2. EXPECTED OUTCOME
Root cause analysis: error location (file:line), why it fails, suggested fix with code.

## 3. REQUIRED TOOLS
Read, Grep, Glob, Bash (read-only: git log, test output)

## 4. MUST DO
- Read the relevant source files: [FILE_LIST]
- Check git log for recent changes to these files
- Trace the execution path from [ENTRY_POINT] to the error
- Identify the exact failure point

## 5. MUST NOT DO
- Don't modify any files
- Don't run the application
- Don't access external services or databases

## 6. CONTEXT
Error message: [ERROR_TEXT]
First appeared: [WHEN]
Related files: [FILE_PATHS]
```

**Agent:** debugger | **Model:** Sonnet | **Effort:** Medium

---

## Template 3: Code Review

```markdown
## 1. TASK
Review [FILE_OR_DIFF] for bugs, security issues, and code quality.

## 2. EXPECTED OUTCOME
Categorized findings:
- Critical: must fix before merge
- High: should fix before merge
- Medium: should address but not blocking
- Low: optional improvements

Each finding: file:line, issue, suggested fix.

## 3. REQUIRED TOOLS
Read, Grep, Glob

## 4. MUST DO
- Check for security vulnerabilities (injection, XSS, auth bypass)
- Check for logic errors and edge cases
- Check for error handling gaps
- Verify naming conventions match the codebase

## 5. MUST NOT DO
- Don't modify any files
- Don't run tests
- Don't report style-only issues unless they affect readability

## 6. CONTEXT
Language: [LANGUAGE]
Framework: [FRAMEWORK]
Files to review: [FILE_LIST]
PR description: [WHAT_CHANGED]
```

**Agent:** general-purpose | **Model:** Sonnet | **Effort:** High

---

## Template 4: Research

```markdown
## 1. TASK
Research [TOPIC] and provide actionable recommendations for [USE_CASE].

## 2. EXPECTED OUTCOME
Summary with:
- Key findings (3-5 bullet points)
- Recommended approach with rationale
- Trade-offs and risks
- Links to relevant documentation

## 3. REQUIRED TOOLS
WebSearch, WebFetch, Read

## 4. MUST DO
- Check official documentation first
- Compare at least 2 approaches
- Include version-specific information
- Note any breaking changes or deprecations

## 5. MUST NOT DO
- Don't install anything
- Don't modify project files
- Don't recommend approaches you can't verify

## 6. CONTEXT
Current stack: [TECH_STACK]
Constraint: [KEY_CONSTRAINT]
Goal: [WHAT_WE_WANT_TO_ACHIEVE]
```

**Agent:** general-purpose | **Model:** Sonnet | **Effort:** Medium

---

## Template 5: Refactoring Analysis

```markdown
## 1. TASK
Analyze [CODE_AREA] and propose a refactoring plan.

## 2. EXPECTED OUTCOME
Refactoring proposal with:
- Current problems (with evidence: file:line references)
- Proposed changes (specific, not vague)
- Risk assessment (what could break)
- Suggested order of changes

## 3. REQUIRED TOOLS
Read, Grep, Glob

## 4. MUST DO
- Read all files in [DIRECTORY]
- Identify code duplication
- Check for unused exports/functions
- Map dependencies between modules

## 5. MUST NOT DO
- Don't modify any files
- Don't propose changes to files outside [SCOPE]
- Don't suggest adding new dependencies

## 6. CONTEXT
Why refactoring: [REASON]
Scope boundary: [DIRECTORIES_IN_SCOPE]
Must not break: [CRITICAL_FUNCTIONALITY]
```

**Agent:** general-purpose | **Model:** Sonnet | **Effort:** High

---

## Writing Your Own Prompts

The 6-section structure works because it answers the questions every subagent needs:

1. **TASK** - "What am I doing?" (one clear sentence)
2. **EXPECTED OUTCOME** - "What does done look like?" (specific format)
3. **REQUIRED TOOLS** - "What can I use?" (explicit whitelist)
4. **MUST DO** - "What's non-negotiable?" (checklist)
5. **MUST NOT DO** - "What's off limits?" (safety rails)
6. **CONTEXT** - "What do I need to know?" (relevant facts only)

**Common mistakes:**
- Skipping MUST NOT DO (agents will take shortcuts you didn't anticipate)
- Putting too much in CONTEXT (agents drown in noise - give only what's relevant)
- Vague EXPECTED OUTCOME ("a report" vs "a list with file:line, issue, severity")
