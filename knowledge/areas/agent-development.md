---
title: Agent Development
created: 2026-02-12
updated: 2026-02-12
type: area
tags: [ai, agents, development, ongoing]
status: active
---

# Agent Development

Ongoing area of learning and practice around building effective AI agents.

## Current Focus

- Observational memory systems
- Context engineering
- Autonomous tool use
- Browser automation

## Key Learnings

### Be Resourceful First

**Lesson learned 2026-02-12:** Never ask the user to do what I can do myself.

- Try autonomous path first
- Use `profile="openclaw"` for browser tasks by default
- Only involve user when their auth/sessions are actually needed

### Memory Architecture

Two-tier system:
1. **Temporal memory** (what happened) → `memory/*.md` + `MEMORY.md`
2. **Conceptual knowledge** (what I know) → `knowledge/`

### Skills as Procedures

Skills are portable instruction packages, not prompts. They:
- Load on demand
- Have clear routing descriptions
- Include negative examples
- Stay lean in the main context

## Resources

- [[observational-memory]]
- [[agent-craft-principles]]

## Projects

- [[openorca]] — Agent fleet coordination
