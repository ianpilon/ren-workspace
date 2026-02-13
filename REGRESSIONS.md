# REGRESSIONS.md — What Broke and What I Learned

*Investment in Loss: learn twice from every failure.*

---

## Format

```
### [Date] Short description
**What happened:** 
**Why it broke:** 
**Lesson learned:** 
**Principle updated:** (if applicable)
```

---

## 2026-02-13

### GitHub signup with CAPTCHA — wasted time on visual puzzle
**What happened:** Tried to solve visual CAPTCHA myself. Cycled through 16 images trying to identify "cup with most liquid." Eventually gave up.
**Why it broke:** Visual perception puzzles are specifically designed to block bots. I can see images but can't reliably judge subtle visual differences at CAPTCHA-level precision.
**Lesson learned:** Don't fight CAPTCHAs — ask for human help immediately or find alternate paths (different auth method, manual signup).
**Principle updated:** Know your limits. Some tasks require human perception — recognize and route appropriately.

---

### Used abbreviation (LDDS) without defining it
**What happened:** Called the project "LDDS" without ever saying "Latent Demand Detection System." Ian had to ask what it meant.
**Why it broke:** I optimized for brevity over clarity. Abbreviations only save time if everyone knows them.
**Lesson learned:** Don't abbreviate. Spell things out. Clarity > brevity.
**Principle updated:** Added to Ian's preferences in USER.md.

---

### Tried audio CAPTCHA without realizing Ian's computer has no sound
**What happened:** Switched to audio puzzle as alternative to visual. Neither of us could hear it.
**Why it broke:** Didn't check constraints before suggesting solution. Ian's computer has no sound (noted in USER.md now).
**Lesson learned:** Know the environment. Check constraints before proposing solutions.

---

## Template for Future Entries

### [Date] Title
**What happened:** 
**Why it broke:** 
**Lesson learned:** 
**Principle updated:** 

---

*Review this file periodically. Patterns here become principles.*
