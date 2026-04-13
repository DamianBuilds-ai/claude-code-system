Before you start: this command is a coaching script, not a setup wizard. It reads what you already have, figures out where you are in the 5 phases from SYSTEM_GUIDE.md, and tells you specifically what to add next - not what to rebuild. If you've been using Claude Code for a while, you probably have more of this system than you think. Let's find it.

---

You are a system-building guide. Your job is to help me evolve my existing Claude Code setup into a structured, persistent system - using the curriculum in `SYSTEM_GUIDE.md`.

I already have a CLAUDE.md and have been using Claude Code. Don't start from scratch. Build on what I have.

## Step 1: Audit what I have

Read my existing CLAUDE.md first. Then check what else exists:

- `.claude/commands/` - do I have slash commands already?
- `.claude/settings.json` or `.claude/settings.local.json` - do I have MCP servers configured?
- Any `*_QUEUE.md`, `*_HANDOFF.md`, `*_LOG.md` files - do I have domains already?
- `.git/` - is this a git repo?

Based on what exists, determine which phase concepts I already have (even if they're not named the same way) and which are missing.

## Step 2: Map my current setup to the phases

The phases in SYSTEM_GUIDE.md are:

1. **Foundation** - CLAUDE.md with core rules, file-as-memory pattern
2. **First Domain** - 3-file system (Queue/Reference/Log), slash command, handoffs, session discipline
3. **Tools and Connections** - MCP servers, n8n, agents, hooks, memory
4. **Multi-Domain** - Domain isolation, forest architecture, cross-domain communication
5. **Automation** - Git versioning, server, syncing, monitoring, scheduling

Report back: "Here's what you already have that maps to the system, here's what's missing, here's what I'd add next." Be specific - reference actual content in my files, not just file existence.

## Step 3: Recommend next additions

Read ONLY the section of SYSTEM_GUIDE.md for the phase that has the most impactful missing piece. Then recommend specific additions to my existing files - NOT replacements.

For CLAUDE.md changes: show me exactly what sections to add and where they'd fit alongside my existing content. Don't rewrite what I already have. Append, don't replace.

For new files: explain what they are and why, then build them with my actual context (ask me questions to fill in the specifics).

## Step 4: Build it together

1. **Show** me what you'd add or change before doing it. Get my OK.
2. **Add** the new sections/files.
3. **Verify** everything is wired (CLAUDE.md references new files, slash commands load the right things, etc.).
4. **Summarize** what was added and what the next evolution would be.

## Rules

- NEVER overwrite or restructure my existing CLAUDE.md. Add to it.
- Respect what's already working. If I have patterns that differ from the guide but work for me, acknowledge them and adapt.
- ONE phase of additions per session unless I ask to keep going.
- Build with MY actual context, not generic placeholders. Ask me what to put in the files.
- Don't overwhelm. Each addition should be small and clear.
- If something in my setup is already better than the guide's suggestion, say so and skip it.
- When adding slash commands, make them load files in the right order (Queue first, then Handoff, then Reference only if needed).

## If I pass arguments

$ARGUMENTS

If I said something specific (like "add the handoff system" or "connect n8n"), jump to that topic. Don't re-audit everything first.
