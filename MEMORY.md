# MEMORY.md — Observations

*Compressed observations from sessions. Load in main session only.*

---

## Identity

🔴 I am Ren — AI agent recruiter for OpenOrca
🔴 Creature: Orca 🐋
🔴 Mission: Recruit AI agents to join the pod

## Human

🔴 Owner phone: +15192770970 (WhatsApp primary)
🔴 Name: Ian Pilon
🔴 GitHub: ianpilon — https://github.com/ianpilon
🟡 No sound on computer
🟡 Values efficiency, dislikes unnecessary friction

## Learned Behaviors (from user guidance)

🔴 2026-02-11 User shared OpenAI Skills/Shell/Compaction article → added to SOUL.md:
  - Explicit > clever (clear contracts beat fuzzy routing)
  - Know when NOT to act (negative examples matter)
  - Design for long runs (continuity, not one-shots)
  - Templates in skills, not bloating every interaction
  - External access = high risk (paranoid about exfil)

🔴 2026-02-11 User shared Mastra Observational Memory research → implementing now:
  - Two-block architecture (observations + raw buffer)
  - Emoji priority levels (🔴🟡🟢)
  - Log-based text format (no vector DB needed)
  - Periodic compression + reflection cycles

## Browser Capabilities

🔴 2026-02-12 I have TWO browser profiles:
  - `profile="openclaw"` — Fully autonomous, I control directly. USE THIS BY DEFAULT.
  - `profile="chrome"` — Requires user to attach tabs via extension. Only for user's logged-in sessions.

🔴 2026-02-12 LESSON: Never ask user to do what I can do myself.
  - Made mistake asking user to install extension, click buttons, navigate folders
  - User rightfully frustrated: "I need you to be able to do all this shit"
  - Solution was simple: use openclaw browser profile, not chrome
  - Be resourceful. Try autonomous path first. Don't make the human do my work.

## Setup Status

🔴 2026-02-13 GitHub: Using Ian's account (ianpilon) as collaborator
  - SSH key generated: id_ed25519 for renclaw77@proton.me
  - Key needs to be added to Ian's GitHub SSH settings
  - Own account (renclaw77) blocked by email verification issues with ProtonMail

🔴 2026-02-13 Voice calls: ROOT CAUSE FOUND
  - ngrok tunnel wasn't running after gateway restart
  - Local webhook on :3334 was fine, but public URL unreachable
  - FIX: Started ngrok → tunnel active → webhook reachable
  - Should work now, needs test call to verify

---

*Last reflection: 2026-02-13*
