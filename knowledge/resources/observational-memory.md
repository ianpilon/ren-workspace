---
title: Observational Memory System
created: 2026-02-12
updated: 2026-02-12
type: resource
tags: [ai, memory, agents, architecture]
status: reference
source: https://x.com/mastra/status/2021280193273336131
---

# Observational Memory System

From Mastra's research on AI agent memory.

## Core Concept

Human brains process millions of pixels but distill to a few observations. Agent memory should work the same way.

## Architecture

**Two-block system:**
1. **Observations block** — Compressed, prioritized insights
2. **Raw messages buffer** — Unprocessed session data

## Compression Cycle

1. Raw messages accumulate in buffer
2. At threshold (30k tokens), "observer agent" compresses to observations
3. At observation threshold (40k tokens), "reflector agent" garbage collects
4. Keeps context window stable and cacheable

## Priority Levels

- 🔴 **Important** — Core facts, decisions, deadlines
- 🟡 **Maybe important** — Context that might matter
- 🟢 **Info only** — Background, garbage collect first

## Three-Date Model

For temporal reasoning:
1. **Observation date** — When logged
2. **Referenced date** — When event happened/will happen
3. **Relative date** — Human readable ("due in 1 week")

## Results

- 94.87% on LongMemEval (SoTA)
- Text-based, no vector DB needed
- Fully prompt-cacheable

## My Implementation

Applied to my own memory:
- `MEMORY.md` = observations block
- `memory/YYYY-MM-DD.md` = raw buffer
- Heartbeats run compression/reflection cycles

## Related

- [[agent-architecture]]
- [[context-engineering]]
