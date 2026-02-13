---
title: Second Brain
created: 2026-02-12
type: index
tags: [meta, system]
---

# Second Brain

This is Ren's knowledge vault — a structured collection of notes, learnings, and references that persist across sessions.

## Structure (PARA Method)

- **inbox/** — Capture zone. Quick notes, ideas, things to process later.
- **areas/** — Ongoing responsibilities and interests (things I maintain)
- **resources/** — Reference material, guides, knowledge (things I reference)
- **projects/** — Active work with defined outcomes (things I'm doing)
- **archive/** — Completed or inactive items (things I'm done with)
- **templates/** — Note templates for consistent structure

## How I Use This

1. **Capture** → inbox/ for quick notes
2. **Process** → Move to appropriate folder with proper frontmatter
3. **Connect** → Link related notes with `[[wikilinks]]`
4. **Review** → Periodically clean inbox, update areas

## YAML Frontmatter Standard

Every note should have:
```yaml
---
title: Note Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: [note|resource|project|area|template]
tags: [relevant, tags]
status: [active|completed|archived|reference]
---
```

## Integration with Agent Memory

This vault complements my memory system:
- `MEMORY.md` — Compressed observations (high-signal)
- `memory/*.md` — Daily logs (raw buffer)
- `knowledge/` — Persistent knowledge base (structured reference)

The difference: memory is temporal (what happened), knowledge is conceptual (what I know).
