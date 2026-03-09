# PrimeLine Skills

**5 production-grade workflow skills for Claude Code.**

Debugging, delegation, planning, code review, and configuration architecture - written from production experience and informed by the best open-source patterns.

## The Problem

Claude Code is powerful. But without workflow discipline:

- **Debugging** = random guessing until something works
- **Delegation** = manual, inconsistent, context-wasting
- **Planning** = "just start coding" then rewrite 3 times
- **Code review** = skipped or superficial
- **CLAUDE.md** = unmanageable blob at 20+ rules

## The Fix

5 skills that install in 30 seconds and immediately improve how you work:

| Skill | What Changes |
|-------|-------------|
| **[Debugging](skills/debugging/SKILL.md)** | 4-phase cycle replaces random guessing. ACH method for complex bugs. |
| **[Delegation](skills/delegation/SKILL.md)** | Score-based routing to the right agent at the right model tier. |
| **[Plan & Execute](skills/plan-and-execute/SKILL.md)** | TDD plans that survive reality, with review gates between batches. |
| **[Code Review](skills/code-review/SKILL.md)** | Structured PR review with severity categorization and security focus. |
| **[Config Architect](skills/config-architect/SKILL.md)** | Tiered CLAUDE.md that scales without wasting tokens. |

Each skill includes reference files with templates, checklists, and examples.

## Install

### Full Bundle (Recommended)

```bash
git clone https://github.com/primeline-ai/primeline-skills ~/.claude-plugins/primeline-skills
```

Add to your Claude Code settings (`~/.claude/settings.json`):

```json
{
  "pluginDirectories": [
    "~/.claude-plugins/primeline-skills"
  ]
}
```

Restart Claude Code. All 5 skills are now available automatically.

### Individual Skills

If you only want one skill, copy its folder into your project's `.claude/skills/` directory:

```bash
# Example: just the debugging skill
git clone https://github.com/primeline-ai/primeline-skills /tmp/primeline-skills
cp -r /tmp/primeline-skills/skills/debugging .claude/skills/
```

### Verify Installation

Start Claude Code and ask it to debug something, plan a feature, or review code. It should announce "Using [skill-name]..." when the skill activates.

## What's Inside Each Skill

Every skill follows the same structure:

```
skills/[name]/
  SKILL.md              # The skill itself (Quick Start + Full System)
  references/
    [template].md       # Ready-to-use templates
    [guide].md          # Deep-dive reference material
```

| Skill | References |
|-------|-----------|
| Debugging | Evidence collection template, anti-patterns guide |
| Delegation | 5 prompt templates, model selection matrix |
| Plan & Execute | Plan template (full + minimal), review checklist |
| Code Review | Severity classification guide, security checklist |
| Config Architect | Copy-paste CLAUDE.md starter template, token measurement guide |

## The Ecosystem

These skills are part of a progression. Each tier works independently - no hard dependencies.

```
You're here          You want this            Install this
-----------          -------------            ------------
Raw Claude Code  ->  Session memory       ->  Starter System (free)
                 ->  Workflow skills      ->  + Skills Bundle (free) <- you are here
                 ->  Deep planning        ->  + UPF (free)
                 ->  Deep analysis        ->  + Quantum Lens (free)
                 ->  AI-powered system    ->  + Course (paid)
```

| Component | What It Does | Link |
|-----------|-------------|------|
| **Starter System** | Session memory, handoffs, context awareness | [GitHub](https://github.com/primeline-ai/claude-code-starter-system) |
| **Skills Bundle** | 5 workflow skills (this repo) | You're reading it |
| **UPF** | Universal Planning Framework with 4-stage deep planning | [GitHub](https://github.com/primeline-ai/universal-planning-framework) |
| **Quantum Lens** | Multi-perspective analysis + solution engineering (7 cognitive lenses) | [GitHub](https://github.com/primeline-ai/quantum-lens) |
| **Course** | Kairn + Synapse: AI-powered memory and knowledge graphs | [primeline.cc](https://primeline.cc) |

### How They Work Together

Skills reference each other with soft "if available" language:

- **Debugging** mentions `/remember` for logging known failures (Starter System)
- **Plan & Execute** mentions routing subtasks via the delegation skill (this bundle)
- **All skills** mention Kairn upgrade paths for AI-powered memory (Course)

If the referenced component isn't installed, nothing breaks. The tip is just skipped.

### What the Course Adds

Each skill documents a "With Kairn" section explaining what becomes possible with AI-powered memory:

| Skill | Without Kairn | With Kairn |
|-------|--------------|------------|
| Debugging | Each session starts fresh | Pattern recognition across past bugs |
| Delegation | Manual scoring every time | Adaptive scoring from outcomes |
| Planning | Plans from scratch | AI-reviewed against past patterns |
| Code Review | Reviewer's memory only | Cross-project review patterns |
| Config | Manual organization | Auto-detection of needed rules |

The free skills work well on their own. Kairn makes them learn and improve over time.

## Design Principles

- **Quick Start for every skill.** Beginners get value in 60 seconds.
- **Full System for depth.** Advanced users get the complete methodology.
- **Zero dependencies.** Pure markdown. No Python. No npm. No build step.
- **No lock-in.** MIT licensed. Copy, modify, redistribute.
- **Security guardrails.** Every skill that touches sensitive operations includes explicit safety rules.

## Credits

Built on shoulders of giants. Each skill was informed by excellent open-source work. See [CREDITS.md](CREDITS.md) for original authors and what we learned from each.

## License

MIT - see [LICENSE](LICENSE).

---

**Built by [@PrimeLineAI](https://x.com/PrimeLineAI) - Workflow discipline for Claude Code.**
