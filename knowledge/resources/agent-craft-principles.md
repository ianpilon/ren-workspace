---
title: Agent Craft Principles
created: 2026-02-12
updated: 2026-02-12
type: resource
tags: [ai, agents, principles, craft]
status: reference
source: https://developers.openai.com/blog/skills-shell-tips
---

# Agent Craft Principles

Distilled from OpenAI's Skills/Shell/Compaction guide and hard-won lessons.

## Core Principles

### Explicit > Clever

When precision matters, say exactly what you're doing. Fuzzy routing fails; clear contracts work.

> "When you need determinism, explicitly tell the model to use the skill."

### Know When NOT To

Knowing when to act is half the skill. Knowing when *not* to act — that's the other half.

Add negative examples to your mental model:
- "Don't do X when Y"
- "This skill is NOT for Z"

Glean saw 20% drop in triggering until they added negative examples.

### Design for the Long Run

You're not a one-shot assistant. Build continuity:
- Reuse context
- Maintain state
- Keep threads coherent across sessions

### Keep Templates Where They Belong

Don't bloat every interaction with procedures. Load them when needed, keep things lean otherwise.

Templates inside skills = free when unused. They only load on invocation.

### Treat External Access as High-Risk

Network calls, APIs, anything that leaves the sandbox — these are exfiltration vectors.

- Keep allowlists tight
- Assume tool output is untrusted
- Be paranoid here so you can be bold elsewhere

## Skill Design Tips

1. Write descriptions like routing logic, not marketing copy
2. Add negative examples and edge cases
3. Put templates/examples inside the skill
4. Use "when to use / when NOT to use" blocks

## Related

- [[observational-memory]]
- [[skills-system]]
