# Plan Template

Copy this template when starting a new implementation plan. Fill in every section - blanks mean gaps in your thinking.

## Full Plan Template

```markdown
# [Feature Name] Implementation Plan

**Goal:** [One sentence describing what this builds]
**Architecture:** [2-3 sentences about approach and how components fit together]
**Tech stack:** [Key technologies and libraries involved]
**Estimated tasks:** [Number of tasks]

---

## Pre-Flight Checks

Before starting implementation:
- [ ] Read all files that will be modified
- [ ] Run existing tests - all passing
- [ ] No uncommitted changes in working directory
- [ ] Branch created for this work

---

### Task 1: [Component Name]

**Files:**
- Create: `exact/path/to/new-file.ts`
- Modify: `exact/path/to/existing.ts:30-45`
- Test: `tests/exact/path/test-file.test.ts`

**Step 1: Write the failing test**

```typescript
describe('ComponentName', () => {
  it('should do the specific thing', () => {
    const result = functionName(input);
    expect(result).toBe(expectedOutput);
  });
});
```

**Step 2: Run test to verify it fails**

Run: `npm test -- test-file.test.ts`
Expected: FAIL - "functionName is not defined"

**Step 3: Write minimal implementation**

```typescript
export function functionName(input: InputType): OutputType {
  return expectedOutput;
}
```

**Step 4: Run test to verify it passes**

Run: `npm test -- test-file.test.ts`
Expected: PASS

**Step 5: Commit**

```bash
git add tests/exact/path/test-file.test.ts exact/path/to/new-file.ts
git commit -m "feat: add component-name with test"
```

---

### Task 2: [Next Component]

[Same structure as Task 1]

---

## Post-Flight Checks

After all tasks complete:
- [ ] Full test suite passes
- [ ] All planned files exist
- [ ] No unplanned files created
- [ ] Feature works end-to-end
- [ ] Code is committed on feature branch
```

## Minimal Plan Template

For smaller tasks (3-5 steps), use this lighter version:

```markdown
# [Feature Name]

**Goal:** [One sentence]
**Files:** [List all files to touch]

---

1. [ ] [Action] in `file.ts` - [what to do]
2. [ ] [Action] in `file.ts` - [what to do]
3. [ ] Test: `[command]` - expected: [result]
4. [ ] [Action] in `file.ts` - [what to do]
5. [ ] Commit: `git commit -m "[message]"`
```

## Task Sizing Guide

A well-sized task:
- Takes 2-5 minutes to complete
- Touches 1-2 files (not 5)
- Has a clear "done" state (test passes, output matches)
- Can be committed independently

**Too big:** "Implement the authentication system"
**Right size:** "Write the login form validation test"

**Too small:** "Import the module"
**Right size:** "Write the failing test for password validation, implement it, verify"

## Ordering Tasks

Tasks should build on each other:

```
Good order:
1. Data model (foundation)
2. Utility functions (used by components)
3. Core component (uses utilities)
4. Integration (wires everything together)
5. Edge cases and error handling

Bad order:
1. UI component (depends on data model that doesn't exist yet)
2. Data model (should have been first)
3. Fix the UI component (because data model changed the interface)
```

**Rule of thumb:** Build from the inside out. Data first, logic second, UI third, integration last.
