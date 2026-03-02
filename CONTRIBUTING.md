# Contributing

Thanks for your interest in contributing to PrimeLine Skills.

## How to Contribute

1. **Bug reports** - Open an issue describing the problem, steps to reproduce, and your environment
2. **Feature requests** - Open an issue describing the use case and proposed solution
3. **Pull requests** - Fork the repo, make your changes, submit a PR

## Guidelines

- Keep it simple. This is a markdown-based project - no build steps, no dependencies
- Test that Claude Code can discover and use your changes via `pluginDirectories`
- Follow the existing file structure and naming conventions
- One change per PR - easier to review, easier to revert if needed
- SKILL.md files should have valid YAML frontmatter (name + description)

## Skill Structure

Each skill follows this pattern:

```
skills/{name}/
  SKILL.md              # Main skill file (YAML frontmatter + markdown)
  references/           # Supporting reference files
    {reference-1}.md
    {reference-2}.md
```

## What We're Looking For

- Bug fixes in skill definitions or references
- Improvements to skill descriptions (better auto-triggering)
- New examples or use cases
- Clarity improvements in reference files

## What We're Not Looking For

- Features that require runtime dependencies (npm, pip, etc.)
- Skills that duplicate existing functionality
- Changes that break backwards compatibility without discussion
- Skills that include learning or adaptive behavior (reserved for paid tier)

## Code of Conduct

This project follows our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold it.
