# Changelog

## v6.1 - 2026-04-21 - Token density + hook audit

Two small additions to `/atlas` based on Stan's real-world adoption case:

- **Token density check:** `/atlas` now computes bytes/line for every file. Files over ~400 bytes/line are flagged as DENSE regardless of line count. A 121-line file at 700 bytes/line weighs ~40K tokens - line count alone misses it.

- **Hook audit:** `/atlas` now scans `~/.claude/hooks/` and any hook directory referenced in `settings.json`. Any SessionStart or UserPromptSubmit hook that `cat`s files into context is flagged as POTENTIAL CONTEXT LEAK, with size of cat'd files shown.

### Why this version exists

Stan paste-migrated Atlas v6.0 and the audit reported his system as lean. His actual init was 30% of context because a hook was silently cat-ing a dense-prose 121-line file (40K tokens) into every session. v6.1 catches that class of leak. Credit to his Opus for finding it.

### Upgrade path

Users on v6.0: paste the migration prompt again. It will read the new VERSION + CHANGELOG, apply the v6.1 delta (just re-running `/atlas` on the updated template), and stamp your CLAUDE.md.

## v6.0 - 2026-04-20 - Builder-friendly QUEUE model
- QUEUE becomes a trunk (200 lines) with on-demand leaves (BACKLOG, ONHOLD, per-project)
- Companion type added (6th domain type) with 600-line combined structural budget
- Principle Zero: progressive loading is the invariant that governs everything else
- 3-stage leaf ladder on trunk: warn 250 / prune 280 / split 300
- No mandatory staleness rule - soft audit prompt only (human decides legitimacy)
- `/atlas` command with two modes: audit (default) and fix (interactive)
- Semantic naming formalised: underscore structural, hyphen on-demand content leaves
- CLAUDE.md family split at 250 lines (PROTOCOLS, INFRASTRUCTURE, BUILDING on-demand)
- Per-doc caps table as single source of truth for all size rules

## v5.0 - Flat QUEUE cap
- 80-line QUEUE cap introduced
- Trim triggers formalised (Recently Completed 5+, QUEUE >80, HANDOFF >3 blocks)
- Type system first draft (5 types, no companion)

## v4.0 - Trunk-and-leaf
- Context routing table pattern introduced in domain trunks
- Leaf split trigger at ~250 lines
- Hyphen vs underscore naming convention established

## v3.0 - Rotation by rhythm
- Per-type LOG rotation naming formalised
- dev: per-project log; ops: per-quarter; journaling: per-month
- HANDOFF 3-block prune rule

## v2.0 - Domain types
- Five domain types declared (dev, ops, journaling, reference, ephemeral)
- Type declaration line in {DOMAIN}.md
- Type-driven rotation rhythm

## v1.0 - 2026-04-20 - Initial draft
- Darius proposal. HTML + prompt + Stan test case.
- Basic 3-file pattern (QUEUE, reference, LOG) documented
- CLAUDE.md as session-start payload established
