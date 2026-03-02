# CLAUDE.md Starter Template

Copy this into your project's `CLAUDE.md` and fill in the blanks. Delete any section that doesn't apply.

---

```markdown
# [Project Name]

## Quick Reference

- **Stack:** [e.g., Next.js 15 + TypeScript + Tailwind]
- **Build:** [e.g., npm run build]
- **Test:** [e.g., npm test]
- **Lint:** [e.g., npm run lint]
- **Dev:** [e.g., npm run dev]

## Project Structure

- Source: [e.g., src/]
- Components: [e.g., src/components/]
- API routes: [e.g., src/app/api/]
- Tests: [e.g., alongside source files as *.test.ts]
- Config: [e.g., .env.local for secrets]

## Core Rules

[3-5 rules that apply to EVERY task. Keep these non-obvious - don't say "write clean code."]

- [e.g., Server components by default. Only add "use client" when component needs browser APIs or state.]
- [e.g., All API input validated with Zod schemas before processing.]
- [e.g., Error responses use the ApiError class from src/lib/errors.ts.]
- [e.g., No default exports except for pages and layouts.]

## Naming Conventions

- Files: [e.g., kebab-case for files, PascalCase for components]
- Variables: [e.g., camelCase, descriptive, no abbreviations]
- Database: [e.g., snake_case for columns, plural for tables]

## Key Patterns

[Patterns Claude should follow when writing code in this project.]

- [e.g., Use the fetch wrapper from src/lib/api.ts for all HTTP calls]
- [e.g., Form validation: Zod schema + React Hook Form + server action]
- [e.g., Database queries through Prisma, never raw SQL]
```

---

## After Filling In the Template

1. **Read it back.** Does every line tell Claude something non-obvious?
2. **Check the size.** Should be under 500 tokens (~50 lines). If longer, move sections to `.claude/rules/`.
3. **Test it.** Ask Claude to do a common task. Does it follow the rules?

## What NOT to Put Here

- Feature requirements (those go in issues or docs)
- Full API documentation (that's Tier 3 - reference docs)
- One-time instructions ("fix the bug in auth.ts")
- Things your linter already enforces
- Obvious practices ("use meaningful variable names")

## Growing Beyond the Template

When your CLAUDE.md needs more than ~50 lines, create Tier 2 rule files:

```
.claude/rules/
  testing.md        # "When writing tests: use vitest, prefer integration tests..."
  api-design.md     # "API routes must: validate input, check auth, return consistent shape..."
  database.md       # "Migrations: use timestamp prefix, one operation per migration..."
```

Claude loads these automatically when the conversation topic matches. See the config-architect SKILL.md for the full tiered loading pattern.
