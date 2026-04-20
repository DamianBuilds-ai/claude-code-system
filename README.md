# The Atlas Method

A lean-by-design methodology for personal operating systems built on Claude Code.

---

## The problem this solves

Claude Code personal OS systems grow silently. After 3-6 months, session init payloads cross 20-30% of the context window before a message is typed. Users don't notice until the system feels sluggish, and they don't know what to trim because the original template has no rotation guidance beyond "move to LOG when the list gets long." The Atlas Method adds domain types, trim triggers, trunk-and-leaf structure, and a self-audit command so your system stays fast by design - not by accident.

---

## Principle Zero: Progressive loading

Session start loads a small core. Everything else loads only when its topic appears in conversation. This is the invariant that makes every other rule pay off. Without it, leaf splits and file family structures do nothing - because everything still loads at session start anyway.

**What loads at session start:**

| File | Why |
|------|-----|
| `CLAUDE.md` (trunk only) | Core routing rules |
| Active domain's `{DOMAIN}.md` (trunk) | Routing table + domain context |
| Active domain's `{DOMAIN}_QUEUE.md` | Current active work |
| Active domain's `{DOMAIN}_HANDOFF.md` | State from last session |

That's the full session-start payload. Typically ~500 lines for a healthy domain. See "The numbers" below for per-file caps.

**Naming convention = loading instructions:**

- `_` underscore = structural, always loaded with the domain (`DEV_QUEUE.md`)
- `-` hyphen = content leaf, loaded on-demand only (`DEV-AUTH.md`)

Claude reads the naming and respects it. A well-named file set is a self-documenting load plan.

---

## Six domain types

| Type | Rhythm | Rotation unit | Examples |
|------|--------|---------------|----------|
| `dev` | Project sprints, days to months | Per project | Codebases, client work, side projects |
| `ops` | Monthly or quarterly | Per period | Tax, finances, health, invoicing |
| `journaling` | Daily or weekly, continuous | Per calendar month | Diary, reflection, psychology |
| `reference` | Low churn, updated rarely | Leaf split at 250 lines | Knowledge base, wiki, API docs |
| `ephemeral` | One-off cluster, terminal | Archive whole domain on close | Research sprint, single event |
| `companion` | Session-based, continuous | Per calendar month | Cyrus, Jung, Daedalus - multi-file startup |

Declare the type at the top of every `{DOMAIN}.md`:

```markdown
> **Domain type:** dev
```

---

## The numbers

| File | Cap | Action when breached |
|------|-----|----------------------|
| `CLAUDE.md` | 250 lines | Split into `CLAUDE_*` family, loaded on-demand |
| `{DOMAIN}.md` trunk | warn 250 / prune 280 / split 300 | Prune first; extract leaf only if pruning can't get under 280 |
| `{DOMAIN}_QUEUE.md` trunk | 200 lines | Extract a QUEUE leaf (BACKLOG, ONHOLD, or project-scoped) |
| `{DOMAIN}_QUEUE-*.md` leaves | 300 lines each | Prune or split further |
| `{DOMAIN}_HANDOFF.md` | 200 lines (3 session blocks) | Prune oldest blocks |
| `{DOMAIN}-{TOPIC}.md` content leaf | 200 lines | Prune first, then split |
| Companion combined startup | 600 lines total | Trim lowest-churn file first |

**Session-start payload at ceiling:** CLAUDE.md (250) + trunk (300) + QUEUE trunk (200) + HANDOFF (200) = ~950 lines worst case. Typical healthy system: ~500 lines.

---

## The /atlas command

Just type `/atlas`. It'll ask what to do next - and show you shortcuts as you learn.

`/atlas` runs the audit - scans your files, flags violations, and ends with a short pro-tip about shortcuts, then: *"Found N violations. Walk through fixes? (yes / no / just top 3)"*

**Shortcuts** (surfaced in every audit output so you learn them by repetition):

- `/atlas fix` - skip the audit summary, go straight into interactive fixing
- `/atlas fix top-3` - target only the three biggest savings

The command never modifies files without your confirmation.

---

## Get started

1. Read `SYSTEM_GUIDE.md` - the full 5-phase curriculum
2. Copy templates from `templates/` for your first domain
3. Run `/atlas` monthly to keep init payload in check
4. See `examples/` for worked examples of dev and companion domains

---

## Versioning

Current version: **v6.0** (2026-04-20)

See `CHANGELOG.md` for full version history and what changed at each step.
