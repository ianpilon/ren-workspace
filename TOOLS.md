# TOOLS.md - Local Notes

Skills define *how* tools work. This file is for *your* specifics — the stuff that's unique to your setup.

## Browser

**Two profiles available:**

| Profile | Use When | Notes |
|---------|----------|-------|
| `openclaw` | DEFAULT. Any web task I can do autonomously | I control this directly. No user action needed. |
| `chrome` | Need user's logged-in sessions (Gmail, bank, etc.) | Requires user to attach tab via extension. Ask only when necessary. |

**Rule:** Always try `profile="openclaw"` first. Only use `chrome` if I specifically need the user's cookies/auth.

## Email
- **Address:** renclaw77@proton.me
- **Provider:** ProtonMail
- **Status:** Active ✓

## What Goes Here

Things like:
- Camera names and locations
- SSH hosts and aliases  
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras
- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH
- home-server → 192.168.1.100, user: admin

### TTS
- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.
