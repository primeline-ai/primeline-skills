# Token Measurement Guide

Understanding how your CLAUDE.md affects token usage helps you make informed decisions about what to keep, move, or delete.

## Token Estimation

Claude uses BPE (Byte Pair Encoding) tokenization. Rough rules:

| Content Type | Tokens per Line (approx) |
|-------------|-------------------------|
| Short instruction | 10-15 |
| Code example | 15-25 |
| Table row | 10-20 |
| Markdown heading | 5-10 |
| Empty line | 1 |

**Quick estimate:** Word count / 0.75 = approximate tokens. A 100-word section is roughly 130 tokens.

## Cost of Tier 1

Your CLAUDE.md (Tier 1) loads on **every single message** in a conversation. This compounds:

| CLAUDE.md Size | Tokens | 50 Messages | 200 Messages |
|---------------|--------|-------------|-------------|
| 1 KB (~250 tokens) | 250 | 12,500 | 50,000 |
| 3 KB (~750 tokens) | 750 | 37,500 | 150,000 |
| 5 KB (~1,250 tokens) | 1,250 | 62,500 | 250,000 |
| 10 KB (~2,500 tokens) | 2,500 | 125,000 | 500,000 |

**Every line in Tier 1 is multiplied by your message count.** This is why keeping Tier 1 lean matters.

## Cost of Tier 2

Rule files in `.claude/rules/` load only when relevant keywords appear in the conversation. Typical loading pattern:

| Rule File | Loads When | Frequency |
|-----------|-----------|-----------|
| testing.md | Writing or discussing tests | ~20% of messages |
| api-design.md | Working on API routes | ~15% of messages |
| database.md | Data model or migration work | ~10% of messages |

A 300-token rule file at 15% loading = 45 tokens average per message instead of 300. That's an 85% reduction vs putting it in Tier 1.

## Measuring Your Config

### Method 1: Character Count (Quick)

```bash
# Tier 1 size
wc -c CLAUDE.md

# Tier 2 total size
wc -c .claude/rules/*.md 2>/dev/null || echo "No Tier 2 files"

# All config combined
find . \( -name "CLAUDE.md" -o -path "./.claude/rules/*.md" \) | xargs wc -c
```

Divide character count by 4 for rough token estimate.

### Method 2: Line Analysis (Detailed)

```bash
# How many lines in Tier 1?
wc -l CLAUDE.md

# What's taking space?
grep -c "^#" CLAUDE.md          # Headings
grep -c "^-" CLAUDE.md          # List items
grep -c '```' CLAUDE.md         # Code block markers
```

## Size Targets

| Tier | Target Size | Max Before Action |
|------|------------|------------------|
| Tier 1 (CLAUDE.md) | 300-500 tokens | 800 tokens |
| Tier 2 (each rule file) | 150-300 tokens | 500 tokens |
| Tier 2 (all combined) | 1,000-2,000 tokens | 3,000 tokens |

**When you exceed the max:**
- Tier 1 over 800 tokens: Move least-used sections to Tier 2
- Tier 2 file over 500 tokens: Split into two focused files
- Tier 2 total over 3,000 tokens: Audit for outdated or redundant rules

## ROI Calculation

Is a rule worth keeping in Tier 1? Calculate its ROI:

```
Value: How often does this rule prevent a mistake?
Cost:  Token count x messages per session

High ROI (keep in Tier 1):
  "All input validated with Zod" - prevents bugs on every API task

Low ROI (move to Tier 2):
  "Database migrations use timestamp prefix" - only matters 5% of the time

Zero ROI (delete):
  "Use meaningful variable names" - Claude does this by default
```

## Common Bloat Sources

| Source | Typical Size | Fix |
|--------|-------------|-----|
| Full API docs inlined | 2,000+ tokens | Move to Tier 3 (docs/) |
| Every convention documented | 1,000+ tokens | Keep top 5, move rest to Tier 2 |
| Historical notes ("we changed X on...") | 200+ tokens | Delete or move to docs |
| Repeated instructions in different words | 300+ tokens | Deduplicate |
| Code examples for every rule | 500+ tokens | One example per rule max |

## Optimization Checklist

- [ ] Tier 1 under 500 tokens
- [ ] No redundant instructions (same rule stated twice in different words)
- [ ] No rules your linter already enforces
- [ ] No one-time instructions that should be in an issue
- [ ] Domain-specific rules in Tier 2, not Tier 1
- [ ] Reference docs in Tier 3, not inlined
- [ ] Every rule in Tier 1 applies to >50% of tasks
