# Plan Review Checklist

Review your plan against this checklist before executing. Every "no" is a gap that will cost you time during execution.

## Completeness

- [ ] **Goal is specific.** "Add dark mode" not "improve the UI."
- [ ] **Every file path is exact.** `src/components/toggle.tsx` not "the toggle component."
- [ ] **Every task has a test** (or explains why testing isn't possible for this step).
- [ ] **Code in the plan is complete.** Not "add validation logic" - the actual validation code.
- [ ] **Commands include expected output.** Not "run the tests" - `npm test -- toggle.test.ts` expecting PASS.
- [ ] **Nothing is missing.** Walk through the feature mentally - does the plan cover everything?

## Dependencies

- [ ] **Tasks are ordered correctly.** No task uses something built in a later task.
- [ ] **No circular dependencies.** Task A doesn't need Task B which needs Task A.
- [ ] **External dependencies are noted.** New packages, API keys, environment variables.
- [ ] **Existing code is read first.** Don't modify files you haven't looked at.

## Sizing

- [ ] **Each task takes 2-5 minutes.** If longer, break it down.
- [ ] **Each task touches 1-2 files.** If more, split it.
- [ ] **Each task has a clear "done" state.** Test passes, output matches, file exists.
- [ ] **Total plan is realistic.** 3-7 tasks for a focused feature, 10-15 for a larger one.

## Safety

- [ ] **No destructive operations** without explicit user approval.
- [ ] **No hardcoded secrets** in the plan (API keys, passwords, tokens).
- [ ] **Existing tests still pass** after each task (not just at the end).
- [ ] **Rollback is possible.** Each task can be reverted via git if something goes wrong.

## Consistency

- [ ] **Naming matches the codebase.** Use the project's conventions, not your preferences.
- [ ] **Patterns match existing code.** If the project uses Context, don't introduce Redux.
- [ ] **File locations follow project structure.** Components go where components go.
- [ ] **Import style matches.** Relative vs absolute, named vs default exports.

## Common Plan Failures

| Failure | Symptom | Prevention |
|---------|---------|-----------|
| Missing dependency | Task 3 fails because Task 2 should have installed a package | List all dependencies in pre-flight |
| Wrong file path | "File not found" during execution | Verify paths exist before planning |
| Oversized task | Task takes 20 minutes, gets confusing | Break into 3-4 smaller tasks |
| Implicit assumption | "This should work" without testing | Every assumption gets a verification step |
| Scope creep mid-plan | Plan grows from 5 tasks to 15 | Stick to the original goal. New ideas go in a follow-up plan |

## Review Workflow

1. **Read the plan header.** Is the goal clear? Does the architecture make sense?
2. **Walk through each task mentally.** Imagine executing each step. Does it work?
3. **Check the checklist above.** Every box should be checked.
4. **Look for the gap.** What did the plan author assume you already know? Is that assumption safe?
5. **If anything fails:** Fix the plan, don't start executing with known gaps.
