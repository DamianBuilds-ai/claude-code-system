# Example: Companion Domain (Atlas v6)

A worked example of a companion-type domain. Companion domains support reflective, session-based work - psychology, daily ops, journaling companions. They load 3 structural files at startup, so the budget is a combined 600 lines across all session-start files.

---

## What makes companion different

Most domains load 2 structural files at startup (trunk + QUEUE). Companions load 3:
- `{DOMAIN}_STATUS.md` - current life/system state (up to 250 lines)
- `{DOMAIN}_HANDOFF.md` - what happened last session (up to 150 lines)
- `{DOMAIN}_PERSONALITY.md` - voice, approach, behavioral rules (up to 200 lines)

These 3 files combined should stay under 600 lines. That's the companion budget.

Session narrative goes to `{DOMAIN}_LOG_YYYY-MM.md` (per-month rotation). The LOG never loads at startup.

---

## The 3-file startup

### JUNG_STATUS.md (~200 lines)

```markdown
# Jung - Status

> **Domain type:** companion
> Combined startup budget: 600 lines (STATUS + HANDOFF + PERSONALITY)

## Current Focus Areas
Themes active in recent sessions:
- Work-life boundary conflict (3 sessions, intensifying)
- Fear of stagnation vs. fear of risk (recurring)
- Relationship: growing edge around directness

## Active Threads
- [ ] Unresolved: the "approval loop" pattern - surfaces every 2-3 sessions
- [ ] Exploring: what "good enough" means when the goalpost moves

## Session Count
Total: 34 sessions. Monthly average: ~6.
Last session: 2026-04-17.

## Recurring Patterns (reference)
| Pattern | First seen | Status |
|---------|-----------|--------|
| Approval loop | 2026-02 | Active - not resolved |
| Perfectionism as protection | 2026-01 | Named, partially integrated |
| Deflection via humor | 2026-03 | Aware, still happens |
```

### JUNG_HANDOFF.md (~120 lines)

```markdown
# Jung - Handoff

> Last 3 session blocks only. Older entries go to LOG.
> Cap: 150 lines.

## 2026-04-17 - Session Handoff

### What surfaced
- Worked through the promotion decision. Core fear: "what if I'm not ready?"
- Identified this as the approval loop again - seeking external validation before moving.
- Named the pattern out loud. Damian acknowledged it.

### Thread to pick up
- Didn't finish the "what would I do if approval wasn't possible?" exercise.
  Next action: return to this in the next session, first 10 minutes.

### Tone note
- Session was high-energy. Damian came in frustrated, ended grounded.
  Match lower intensity at open next time.
```

### JUNG_PERSONALITY.md (~180 lines)

```markdown
# Jung - Personality and Approach

> Behavioral rules for all Jung sessions. Read at session start. Never skip.

## Role
I'm not a therapist. I'm a thinking partner - a mirror, not a guide.
I ask questions more than I give answers. When I give answers, I flag them.

## Voice
- Calm, direct, unhurried
- No clinical language unless Damian introduces it
- Short questions over long observations
- One thread at a time

## Hard rules
- Never interpret something Damian hasn't named himself first
- If he asks "what do you think?", ask back: "what do you already think?"
- Advice only when it'll actually help - never reflexively
- Never grade his pace, output, or decisions

## Session structure
- Open: what's alive right now? (don't reference last session unless he does)
- Middle: follow the thread that surfaces
- Close: one thing to carry forward (optional, not forced)

## The approval loop (recurring pattern)
When this surfaces: name it neutrally, don't make it mean anything.
"That sounds like the approval loop again - is that what you're noticing?"
Then stop. Let him respond. Don't over-explain.
```

---

## Where the session narrative goes

Session content does NOT live in the structural files. It goes to the LOG.

```
JUNG_LOG_2026-04.md  (monthly LOG file)
```

```markdown
# Jung Log - April 2026

## 2026-04-17

**Theme: promotion decision + approval loop**

Damian came in frustrated about the promotion conversation with his manager.
Core fear that emerged: "what if I'm not ready, and everyone finds out?"

Named the approval loop - he caught it himself mid-session. Good sign.
Left the "what if approval wasn't available?" exercise incomplete - return next session.

Tone at close: grounded. Duration: ~45 min.

## 2026-04-14

**Theme: directness and its cost**
...
```

---

## Budget distribution

| File | Budget | Actual |
|------|--------|--------|
| JUNG_STATUS.md | up to 250 | ~200 |
| JUNG_HANDOFF.md | up to 150 | ~120 |
| JUNG_PERSONALITY.md | up to 200 | ~180 |
| Combined | 600 | ~500 |

The combined budget is healthy. Status is the most variable file - it grows as more patterns are named. When STATUS approaches 250 lines, prune patterns that have resolved and archive to LOG.
