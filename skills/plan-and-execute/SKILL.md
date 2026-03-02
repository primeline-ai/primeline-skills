---
name: plan-and-execute
description: Use when you have a multi-step task, before touching code. Creates bite-sized implementation plans with TDD emphasis, then executes them in batches with review checkpoints. Prevents the "just start coding" trap.
---

# Plan and Execute

Write the plan before writing the code. Execute it in small batches. Review between batches. This is how you avoid rewriting the same feature three times.

**Announce:** "Using plan-and-execute to structure this implementation."

## Quick Start

For tasks under 30 minutes:

1. **List the steps** (3-7 concrete actions)
2. **Do them in order** (one at a time, verify each)
3. **Commit after each working state**

If the task is bigger than 30 minutes, use the full system below.

## The Full System

### Phase 1: Write the Plan

Every plan starts with a header:

```markdown
# [Feature Name] Implementation Plan

**Goal:** [One sentence - what this builds]
**Architecture:** [2-3 sentences - how it fits together]
**Files involved:** [List of files to create or modify]

---
```

Then break it into tasks. Each task follows this structure:

```markdown
### Task 1: [Component Name]

**Files:**
- Create: `src/components/widget.tsx`
- Modify: `src/app/page.tsx:45-60`
- Test: `tests/widget.test.ts`

**Step 1: Write the failing test**
[Exact test code]

**Step 2: Run it to confirm it fails**
Run: `npm test -- widget.test.ts`
Expected: FAIL - "widget is not defined"

**Step 3: Write the implementation**
[Exact implementation code]

**Step 4: Run the test to confirm it passes**
Run: `npm test -- widget.test.ts`
Expected: PASS

**Step 5: Commit**
git add src/components/widget.tsx tests/widget.test.ts
git commit -m "feat: add widget component"
```

#### Plan Rules

- **Exact file paths.** Not "the component file" - the actual path.
- **Complete code.** Not "add validation" - the actual code to write.
- **Exact commands.** Not "run the tests" - the actual command with expected output.
- **One thing per task.** If a task touches 5 files, it's 3 tasks.
- **2-5 minutes per step.** If a step takes longer, break it down further.

### Phase 2: Review Before Executing

Before writing any code, review your plan:

```
Review Checklist:
- [ ] Every task has exact file paths
- [ ] Every task has a test (or explains why not)
- [ ] No task depends on something not yet built
- [ ] Tasks are ordered so each builds on the last
- [ ] The plan covers the full feature (nothing missing)
- [ ] Nothing in the plan contradicts existing code
```

If you find a gap, fix the plan first. Never start executing a plan you're not confident in.

### Phase 3: Execute in Batches

**Default batch size: 3 tasks.**

For each task in the batch:
1. Mark it as in_progress
2. Follow each step exactly as written
3. Run verifications at each step
4. Mark as completed when all steps pass

After each batch, stop and report:
- What was implemented
- What verification output showed
- Any deviations from the plan (and why)

Then: "Ready for feedback before continuing."

### Phase 4: Adjust and Continue

After feedback:
- Apply requested changes
- Update the plan if scope changed
- Execute the next batch
- Repeat until all tasks are done

### Phase 5: Final Verification

After all tasks:
- Run the full test suite
- Check that all planned files exist
- Verify no unplanned changes were made
- Confirm the goal from the plan header is met

## When to Use

- Feature with 3+ files to create or modify
- Refactoring that spans multiple modules
- Any task where "just start coding" has bitten you before
- When you need to delegate subtasks (the plan becomes the delegation brief)
- When you want review checkpoints, not a surprise PR

## When NOT to Use

- Single-file changes with obvious implementation
- Bug fixes where you already know the cause and fix
- Tasks under 10 minutes
- Exploratory work where you don't know what you're building yet (brainstorm first)

## Quick DSV (Decision Check)

Before executing any plan, run 3 questions in 30 seconds:

1. **What are the 2-3 key claims in this plan?** (What does it assume is true?)
2. **What alternative approach haven't I considered?** (Is there a simpler way?)
3. **Which assumption am I least sure about?** (Validate that first.)

If question 2 reveals a genuinely simpler approach, update the plan before starting.

## Examples

### Example 1: Small Feature Plan

```markdown
# Add Dark Mode Toggle

**Goal:** Users can switch between light and dark themes.
**Architecture:** CSS variables for theming, React context for state, localStorage for persistence.
**Files involved:** src/styles/theme.css, src/context/theme.tsx, src/components/theme-toggle.tsx

---

### Task 1: Theme CSS Variables
**Files:** Create: src/styles/theme.css
**Steps:** Define light/dark variable sets, add to global styles.

### Task 2: Theme Context
**Files:** Create: src/context/theme.tsx, Test: tests/theme.test.tsx
**Steps:** TDD - write test for toggle behavior, implement context with localStorage.

### Task 3: Toggle Component
**Files:** Create: src/components/theme-toggle.tsx, Modify: src/app/layout.tsx
**Steps:** TDD - write test for rendering, implement button, wire into layout.
```

### Example 2: Handling Plan Deviations

During execution, you discover the CSS variables approach conflicts with the existing Tailwind setup.

**Don't:** Silently switch approaches and keep going.

**Do:** Stop the batch. Report:
```
Deviation: CSS variables conflict with Tailwind's dark mode utility classes.
Options:
A) Use Tailwind's built-in dark: prefix (simpler, less CSS)
B) Override Tailwind's config to use CSS variables (more control, more setup)

Recommendation: Option A - aligns with existing patterns.
Waiting for feedback before continuing.
```

## Delegation Integration

Plans make delegation natural. Each task in your plan can become a delegation prompt:

- Independent tasks? Run them as parallel subagents.
- Complex task? Delegate investigation, keep the implementation.
- Review needed? Delegate to a review agent between batches.

See the **smart-delegation** skill for prompt templates and model routing.

## Anti-Patterns

| Pattern | Problem | Fix |
|---------|---------|-----|
| "Just start coding" | No plan = rework 3 times | Write the plan first, even a 5-line one |
| Giant tasks | "Implement auth system" is 10 tasks, not 1 | Break down until each task is 2-5 minutes |
| Vague steps | "Add validation" without specifying what | Exact code, exact file paths, exact commands |
| No verification | Assuming code works without running it | Every task ends with a test or verification command |
| Silent deviations | Changing the approach without reporting | Stop, report deviation, wait for feedback |
| Planning everything | 2-day plan for a 10-minute task | Use Quick Start for small tasks |
| No tests in plan | "We'll test later" | TDD: write the test first, then the code |

## Blocker Protocol

When you hit a blocker during execution:

1. **Stop.** Don't try to work around it silently.
2. **Document** what you attempted and what failed.
3. **Report** the blocker with context: what's blocked, why, what you need.
4. **Wait** for guidance. Don't guess past blockers.

If the blocker is in your own code: use the **systematic-debugging** skill.

## With Starter System

If you have the [Starter System](https://github.com/primeline-ai/claude-code-starter-system) installed:
- Use `/handoff` to save plan progress between sessions
- Plans survive session boundaries through handoff files
- Use `/remember` to log plan patterns that work well for your project

## With Kairn (Course)

When [Kairn](https://primeline.cc) is configured, planning gains:
- **AI-reviewed plans** - Kairn checks plans against past project patterns before execution
- **Blocker detection** - "This task structure failed 3 times before - try this instead"
- **Plan templates** - learns which plan structures work for your codebase

Without Kairn: plans are manual and each session starts fresh. The plan-and-execute cycle still works, you just build institutional knowledge manually.
