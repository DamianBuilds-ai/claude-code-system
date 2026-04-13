# Claude Code: From Tool to System

I spent the first few months using Claude Code the same way most people do - open a session, ask for something, close the tab, repeat. It worked. It was fine. But every new session started from zero.

This guide is about fixing that. Not by hacking Claude's memory, but by building the infrastructure around it so that sessions become interchangeable. The AI changes. The files stay.

These five phases are how I built that infrastructure for myself. You won't need all of them on day one. But they compound, so don't skip ahead.

---

## How to use this guide

Read sequentially. Work one phase until it's solid before moving to the next. The phases reference each other and the concepts build.

| Phase | What you build | When |
|-------|---------------|------|
| 1. Foundation | Your CLAUDE.md, first rules, the mental shift | Day 1 |
| 2. First Domain | The 3-file system, a slash command, handoffs | Week 1 |
| 3. Tools and Connections | MCP servers, n8n, agents, hooks | Week 2 |
| 4. Multi-Domain | Domain isolation, the forest architecture | Month 1 |
| 5. Automation | Git versioning, scheduling, monitoring | Month 2+ |

**Real-world case study:** Throughout this guide I'll reference a system called Oxide - 20+ domains managing business, finances, health, content, gaming, and infrastructure through Claude Code. I built it over roughly nine months. You won't build that on day one, but every piece of Oxide started with Phase 1.

---

# Phase 1: The Foundation

## The mental shift

Here's the thing nobody explains when you start using Claude Code seriously: sessions are stateless. When you close one, the AI forgets everything. Decisions, context, what was in progress, what failed - gone. You start over.

The way out of this isn't better prompting. It's a different mental model.

Instead of treating Claude Code as something you talk to, treat it as a system that reads and writes files. The files hold the state. The AI is the engine that processes them.

```text
Before: You <-> Claude (conversation dies when session ends)
After:  You <-> Claude <-> Files (files persist, sessions are temporary)
```

This one shift is what everything else builds on. Sessions are ephemeral. Files are permanent. Build your system in files, and every session picks up exactly where the last one left off.

## CLAUDE.md - your system's operating manual

When Claude Code starts in a project directory, it automatically reads a file called `CLAUDE.md` in the project root. Whatever's in there shapes how Claude behaves in every session.

Think of it as the constitution of your system. Every session reads it. Every session follows it.

**Where CLAUDE.md files live:**

| Location | Scope | Use Case |
|----------|-------|----------|
| `./CLAUDE.md` | This project only | Project-specific rules, domain structure, file paths |
| `~/.claude/CLAUDE.md` | All projects | Personal preferences, global rules |

Start with one `CLAUDE.md` in your project root. You can add a global one later.

## Your first CLAUDE.md

Create a new directory for your system. This is where everything will live.

```bash
mkdir ~/my-system
cd ~/my-system
git init
```

Now create your CLAUDE.md:

```markdown
# CLAUDE.md - System Instructions

## Who I Am
[Your name]. I'm building a system to manage my [business/life/projects].
I know [your background - e.g., intermediate coding, familiar with n8n].

## Core Rules

1. **Read before acting.** Always read the relevant files before doing work.
2. **Save progress constantly.** Update files after completing any task.
   Never rely on conversation memory.
3. **Ask before assuming.** If unclear, ask. Don't guess.
4. **Be concise.** Skip preambles. Max 3 sentences for simple answers.
5. **Do what's asked. Nothing more.** Mention related ideas,
   but ask before acting on them.

## File Structure
[You'll fill this in as you build - leave it as a placeholder for now]

## Key Paths
| Path | Purpose |
|------|---------|
| `~/my-system/` | Project root |
```

That's it for now. Keep it small. It will grow. Every rule you add should come from actual experience - you'll discover what Claude needs to be told as you work with it. The CLAUDE.md I run now is several hundred lines. None of that was written upfront.

**The principle:** CLAUDE.md grows organically. Don't write the perfect one upfront. Add rules when Claude does something you don't want, or misses something you expected. Each rule is a lesson learned.

## How Claude Code reads your system

Claude Code has built-in tools for interacting with your filesystem:

| Tool | What It Does |
|------|-------------|
| **Read** | Reads file contents |
| **Write** | Creates new files |
| **Edit** | Modifies existing files (surgical replacements) |
| **Glob** | Finds files by name pattern |
| **Grep** | Searches file contents |
| **Bash** | Runs shell commands |

When you tell Claude to "check my queue" or "update the task list," it uses these tools to read and write your markdown files. You don't manage this directly - just know that Claude interacts with your files the same way you would, just faster.

## Settings and permissions

Claude Code has a settings file that controls permissions and connected tools:

```text
~/.claude/settings.json      # Global settings (all projects)
.claude/settings.json         # Project settings (git-tracked)
.claude/settings.local.json   # Project settings (git-ignored, for secrets)
```

You'll configure these more in Phase 3 when you add MCP servers. For now, the defaults work fine. Claude will ask permission before running commands.

**Tip:** When Claude asks to run a tool and you trust it, you can allow it for the session or permanently. Start conservative - allow per-session until you're comfortable, then set permanent allows for tools you use constantly.

## Why files are memory

This deserves emphasis because it's the foundation of everything that follows.

Claude Code sessions are stateless. When you close a session, the AI forgets everything. But your files don't. So:

- **Decisions** get written to files, not just discussed
- **Progress** gets tracked in files, not just remembered
- **Context** gets stored in files, not just explained

Every phase after this builds on this principle. The 3-file system (Phase 2) is a pattern for organising what to remember. Handoffs (Phase 2) are a pattern for session continuity. Slash commands (Phase 2) are a pattern for loading the right context. All of it is files.

---

# Phase 2: Your First Domain

## What is a domain?

A domain is an area of responsibility - a distinct part of your business or life that has its own context, tasks, and history.

Examples:
- **Clients** - proposals, onboarding, relationship management
- **Marketing** - content, social media, email campaigns
- **Finances** - invoicing, tax, bookkeeping
- **Product** - your app or service development
- **Health** - fitness, nutrition, appointments
- **Learning** - courses, skills, certifications

Start with ONE domain. The one that will benefit most from persistent AI assistance. For most people starting with business automation, that's their core business operations.

## The 3-file pattern

Every domain uses three files:

### 1. Queue (BUSINESS_QUEUE.md)
*"What am I working on right now?"*

The active task list. This is the most important file - Claude reads it first every session. It contains:
- **Quick Resume** - a 2-3 line summary of current state (what's active, what's next)
- **Active tasks** - checkboxes, grouped logically
- **Recently Completed** - last few finished items (rotates to Log)

```markdown
# Business - Queue and Active Work

## Quick Resume
Building client onboarding workflow. CRM is set up.
Proposal template done. Next: automate follow-ups.

---

## Queue

### Client Onboarding
- [x] Set up CRM fields for new clients
- [x] Write proposal template
- [ ] Build automated follow-up sequence
- [ ] Create welcome email template
- [ ] Set up project kickoff checklist

### Lead Generation
- [ ] Research 10 potential clients in [niche]
- [ ] Draft cold outreach template
- [ ] Set up lead tracking spreadsheet

---

## Recently Completed
- 2026-04-08: CRM configured with custom fields
- 2026-04-07: Proposal template v1 written and tested
```

### 2. Reference (BUSINESS.md)
*"How does this domain work?"*

The knowledge base. Architecture, context, rules, patterns. Not read every session - only when Claude needs deeper context.

```markdown
# Business - Reference

## Overview
[What your business does, who you serve, how you operate]

## Tools and Services
| Tool | Purpose | Access |
|------|---------|--------|
| CRM | Client tracking | crm.example.com |
| Invoicing | Billing | app.invoicetool.com |
| Email | Outreach | Listmonk / Mailchimp / etc. |

## Processes
### Client Onboarding
1. Proposal sent and accepted
2. Welcome email triggered
3. Project kickoff meeting scheduled
4. Workspace set up
5. First deliverable timeline confirmed

## Pricing
[Your pricing structure, packages, etc.]

## Notes
[Anything Claude should know about how you run your business]
```

### 3. Log (BUSINESS_LOG.md)
*"What did I do before?"*

The history. Completed tasks rotate here from the queue's "Recently Completed" section. Rarely read unless you need to look back.

```markdown
# Business - Log

## 2026-04-08
### CRM Setup
- Configured custom fields: company, contact, project status, value
- Set up pipeline stages: Lead > Proposal > Active > Complete
- Added tags for service categories

## 2026-04-07
### Proposal Template
- Drafted template with scope, timeline, pricing sections
- Added terms and conditions
- Tested with sample client data
```

**The rotation rule:** When "Recently Completed" in the Queue has 5+ items, move the oldest ones to the Log. This keeps the Queue focused on what's current.

### Bonus: Handoff (BUSINESS_HANDOFF.md)
*"What does the next session need to know?"*

The relay baton. Written at the end of every session. Read at the start of the next one.

```markdown
# Business - Handoff

## Last Session: 2026-04-10

### What Was Done
- Built automated follow-up sequence (3 emails, 2-day intervals)
- Tested with dummy client data - working

### In Progress
- Welcome email template (50% done, needs design review)

### Blocked
- Lead tracking needs API access to [platform] - waiting on approval

### Context for Next Session
- Follow-up sequence uses n8n workflow ID: abc123
- Email template draft is in ~/my-system/drafts/welcome-v1.md
- Client asked about rush pricing - need to decide on policy
```

## Building your first domain

Here's the practical exercise. Create your first domain:

```bash
# From your project root
touch BUSINESS_QUEUE.md
touch BUSINESS.md
touch BUSINESS_LOG.md
touch BUSINESS_HANDOFF.md
```

Fill in the Queue with your actual current tasks. Fill in the Reference with your actual business context. The Log and Handoff start empty - they'll fill naturally.

**Update your CLAUDE.md** to tell Claude about this structure:

```markdown
## File Structure
Every domain has up to 4 files:
- `{DOMAIN}_QUEUE.md` - Active work (read FIRST every session)
- `{DOMAIN}.md` - Reference/knowledge base (read when needed)
- `{DOMAIN}_LOG.md` - History (rarely read)
- `{DOMAIN}_HANDOFF.md` - Session relay baton (read at session start)

## Session Rules
1. Read the QUEUE file first. Always.
2. Read the HANDOFF file to see what happened last session.
3. Do the work.
4. Update the QUEUE (check off completed, add new tasks).
5. Write the HANDOFF (what was done, what's in progress, what's blocked).
```

## Slash commands - domain entry points

Slash commands are custom prompts you invoke with `/commandname`. They load specific context and instructions for Claude.

**Where they live:**

```text
~/.claude/commands/        # Available in all projects (user-level)
.claude/commands/          # Available in this project only
```

Create your first slash command:

```bash
mkdir -p .claude/commands
```

Create `.claude/commands/business.md`:

```markdown
You are helping me manage my business operations.

## Loading Order
1. Read `BUSINESS_QUEUE.md` first - this is the active work queue.
2. Read `BUSINESS_HANDOFF.md` - what happened last session.
3. Read `BUSINESS.md` only if you need deeper context.

## Rules
- Update the QUEUE after completing any task.
- Write a HANDOFF entry at session end.
- One task at a time. Finish it before starting the next.
- Save progress constantly. Don't rely on conversation memory.

## What You Help With
- Client management and follow-ups
- Proposal drafting and sending
- Process automation
- Business planning and strategy

ARGUMENTS: $ARGUMENTS
```

Now when you type `/business` in Claude Code, it loads this context automatically. You can also pass arguments: `/business draft a proposal for the new client` and `$ARGUMENTS` gets replaced with "draft a proposal for the new client."

**Why this matters:** Without slash commands, you explain context every session. With them, you type `/business` and Claude already knows what to read, how to behave, and what you're working on. That time-to-productive goes from 5 minutes to about 30 seconds.

## The handoff system

Each Claude Code session has no memory of previous ones. The Handoff file is the bridge.

**End of every session:**
1. Update the Queue (check off what's done, add new tasks)
2. Write the Handoff (what happened, what's next, what's blocked)

**Start of every session:**
1. Read the Queue (what am I working on?)
2. Read the Handoff (what happened last time?)

This creates continuity. The next session picks up exactly where you left off - like a relay runner passing the baton. The runner changes, but the race continues.

**Add this to your CLAUDE.md:**

```markdown
## Session End Protocol
When I say "wrap up":
1. Update QUEUE - check off completed items, add new tasks
2. Write HANDOFF - what was done, in progress, blocked, context for next session
3. Confirm what was saved
```

## Session discipline

The system only works if you follow the discipline:

1. **Start with the slash command.** `/business` not "hey, help me with my business stuff."
2. **Let Claude read the files.** Don't explain what's in them - let it read.
3. **Work one task at a time.** Finish and check off before starting the next.
4. **Wrap up properly.** Never close a session without updating Queue and Handoff.

This feels rigid at first. Within a week it becomes automatic. And the payoff is real - every session is productive from minute one because the context is already there.

---

# Phase 3: Tools and Connections

## MCP servers - connecting external tools

MCP (Model Context Protocol) servers let Claude Code interact with external services directly. Instead of you copy-pasting data between tools, Claude can read from and write to them.

**What MCP servers do:**
- Give Claude new tools (like "search the web" or "list n8n workflows")
- Connect Claude to databases, APIs, and services
- Run as background processes that Claude communicates with

**How to configure them:**

Add MCP servers to your settings file. The location depends on scope:

```text
~/.claude/settings.json           # Available in all projects
.claude/settings.json             # This project only
.claude/settings.local.json       # This project, not tracked by git (use for secrets)
```

Example structure:

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "package-name"],
      "env": {
        "API_KEY": "your-key-here"
      }
    }
  }
}
```

Each MCP server gives Claude a set of tools. Once configured, Claude uses them like its built-in tools (Read, Write, Bash, etc.).

## n8n integration

If you use n8n for workflow automation, this is where things get interesting. The n8n MCP server lets Claude Code manage your workflows directly.

### Setting up n8n MCP

Install the `n8n-mcp` npm package globally:

```bash
npm install -g n8n-mcp
```

Find the install path of the `dist/mcp/index.js` entry point (usually under your global node_modules). Then add to your Claude Code settings file:

```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "node",
      "args": ["/path/to/node_modules/n8n-mcp/dist/mcp/index.js"]
    }
  }
}
```

The package reads your n8n API key from `~/.api_keys` or environment variables, not from the MCP config. Check the [n8n-mcp README](https://www.npmjs.com/package/n8n-mcp) for the current auth approach.

### What you can do with n8n from Claude Code

Once connected, Claude can:

| Action | What It Does |
|--------|-------------|
| List workflows | See all your n8n workflows |
| Get workflow | Read a specific workflow's configuration |
| Create workflow | Build new workflows from a description |
| Update workflow | Modify existing workflow nodes and connections |
| Test workflow | Execute a workflow and see results |
| Check health | Verify n8n is running |

**Example interactions:**

```text
You: "Show me all my active workflows"
Claude: [uses n8n MCP to list workflows, shows you the results]

You: "Create a workflow that sends me a Telegram message every morning with my tasks"
Claude: [uses n8n MCP to build the workflow, returns the workflow ID]

You: "The email notification workflow isn't firing - debug it"
Claude: [reads the workflow config, checks execution history, identifies the issue]
```

### n8n best practices

Add these to your CLAUDE.md once you're working with n8n:

```markdown
## n8n Rules
- Never delete workflows. Deactivate/archive instead. They contain logic and history.
- Fetch workflow code fresh from the API. Don't store workflow JSON in markdown.
- After building or modifying a workflow: "needs testing to confirm."
- Always confirm scope and check pricing before bulk API operations.
```

**Why "never delete":** Workflows contain prompts, logic, and configuration that took time to build. Deactivating preserves them. Deleting loses them forever. I have workflows from months ago that I've come back to - you don't know what'll be useful later.

## Agents - delegating to sub-agents

Claude Code can spawn sub-agents - separate Claude instances that work independently with their own context window. Useful for:

- **Parallel research** - multiple agents searching for different things simultaneously
- **Keeping main context clean** - heavy file reading happens in the agent, not your conversation
- **Different model routing** - faster, cheaper models for simple tasks

### How agents work

```text
Your session (main context)
  |
  |-- Agent 1: "Search the codebase for all API endpoints" (fast model)
  |-- Agent 2: "Research competitor pricing" (balanced model)
  |-- Agent 3: "Analyse this complex architecture" (full model)
  |
  Results come back as summaries into your main context
```

Each agent gets its own context window. It does its work and returns a result. Your main session stays clean.

**When to use agents:**
- Reading 3+ files (keeps the content out of your main context)
- Research tasks (web searches, doc lookups)
- Parallel independent tasks
- Work that doesn't need your input mid-way

**When not to use agents:**
- Simple questions you can answer directly
- Tasks that need back-and-forth with you
- When you need to see every detail (agents summarise)

### Agent model routing

Different tasks need different levels of intelligence:

| Model | Use for | Cost/Speed |
|-------|---------|-----------|
| Haiku | Quick lookups, file discovery, simple searches | Fast, cheap |
| Sonnet | Research, doc analysis, simple edits, code review | Balanced |
| Opus | Complex debugging, architecture analysis, creative work | Slow, powerful |

**Add to CLAUDE.md:**

```markdown
## Agent Delegation
- 3+ file reads -> prefer an agent (keeps main context clean)
- Quick lookups -> haiku agent
- Research and analysis -> sonnet agent
- Complex reasoning -> opus agent (use sparingly)
- Always summarise agent results concisely. Don't parrot everything back.
```

## Hooks - automated behaviours

Hooks are shell commands that run automatically when certain events happen in Claude Code. Configured in your settings file.

**Example use cases:**
- Run a linter after every file edit
- Log session timestamps
- Validate changes before committing
- Inject current date/time into every prompt

**Configuration in settings.json:**

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"Current datetime: $(date '+%A, %B %d %Y - %I:%M %p')\"",
            "timeout": 5000
          }
        ]
      }
    ]
  }
}
```

This example injects the current date and time into every prompt. Claude doesn't inherently know what day it is - this fixes that.

**Hook events you can use:**

| Event | When It Fires |
|-------|--------------|
| `UserPromptSubmit` | When you send a message |
| `PreToolUse` | Before a tool runs |
| `PostToolUse` | After a tool runs |
| `Stop` | When Claude stops responding |

**Start simple.** A datetime hook is a good first hook. Add more as you discover repetitive checks you want automated.

## Memory - persistent context across conversations

Claude Code has a built-in memory system that persists across conversations within a project. It stores memories in `~/.claude/projects/{project-path}/memory/`.

**How it works:**
- Claude can save memories (facts about you, your preferences, project context)
- Memories are loaded into future conversations automatically
- An index file (`MEMORY.md`) tracks what's stored

**Types of memory:**
- **User** - your role, preferences, knowledge level
- **Feedback** - corrections and confirmations of approach
- **Project** - ongoing work, goals, deadlines
- **Reference** - pointers to external resources

**When to use memory vs files:**
- **Memory** = things useful across many future conversations (your preferences, corrections)
- **Domain files** = structured project state (tasks, handoffs, reference docs)

Memory is supplementary to your file system, not a replacement. Your Queue, Reference, Log, and Handoff files are the primary system. Memory fills in the personal gaps.

---

# Phase 4: Multi-Domain Architecture

## When to split into multiple domains

You started with one domain. At some point you'll notice:
- Your Queue file is getting cluttered with unrelated tasks
- You're loading context you don't need for the current task
- Different areas have different tools, different rules, different rhythms

That's when you split.

**Rule of thumb:** If two areas of work have different tools, different contexts, and different cadences, they're different domains.

Example split:
```text
Before: BUSINESS_QUEUE.md (contains everything)

After:
  CLIENTS_QUEUE.md    (client management, proposals, onboarding)
  MARKETING_QUEUE.md  (content, social media, outreach)
  FINANCES_QUEUE.md   (invoicing, tax, bookkeeping)
```

**Don't split too early.** One cluttered domain is better than three empty ones. Split when the clutter actively slows you down.

## Domain isolation

Once you have multiple domains, isolation matters. Each domain should load only its own files.

**Why:**
- **Context limits.** Claude has a context window. Loading all domains wastes it.
- **Focus.** Irrelevant information leads to irrelevant suggestions.
- **Speed.** Less to read means faster startup.

**Add to your CLAUDE.md:**

```markdown
## Domain Isolation

When working on a domain, read ONLY that domain's files.

| Working on... | Read these ONLY |
|---------------|----------------|
| Clients | CLIENTS_QUEUE.md, CLIENTS.md |
| Marketing | MARKETING_QUEUE.md, MARKETING.md |
| Finances | FINANCES_QUEUE.md, FINANCES.md |

Cross-domain work: If the task spans multiple domains, read both.
If unsure which domain: ASK. Don't explore multiple domains to figure it out.
```

Each domain gets its own slash command:

```text
.claude/commands/clients.md
.claude/commands/marketing.md
.claude/commands/finances.md
```

Each command tells Claude exactly which files to read and what rules to follow for that domain.

## The forest architecture

As your system grows, it helps to have a mental model. Here's the one I use:

```text
CLAUDE.md                          <- THE SOIL (always loaded, global rules)
  |
  |-- CLIENTS.md                   <- TRUNK (domain reference)
  |     |-- CLIENTS_QUEUE.md       <- BRANCH (active work)
  |     |-- CLIENTS_LOG.md         <- BRANCH (history)
  |     |-- CLIENTS_HANDOFF.md     <- BRANCH (session relay)
  |     |-- CLIENTS_ONBOARDING.md  <- LEAF (specialised sub-doc)
  |
  |-- MARKETING.md                 <- TRUNK
  |     |-- MARKETING_QUEUE.md     <- BRANCH
  |     |-- MARKETING_LOG.md       <- BRANCH
  |     |-- MARKETING_CONTENT.md   <- LEAF
  |
  |-- FINANCES.md                  <- TRUNK
        |-- FINANCES_QUEUE.md      <- BRANCH
        |-- FINANCES_TAX.md        <- LEAF
```

**The metaphor:**
- **Soil** = CLAUDE.md. Always present. Nourishes every tree with behavioural rules.
- **Trunk** = Domain reference doc. The main knowledge base for that area.
- **Branches** = Queue, Log, Handoff. Supporting docs that change often.
- **Leaves** = Specialised sub-docs. When a topic gets too big for the trunk, it becomes a leaf.

**Growing rules:**
- Keep leaves under ~300 lines. If a leaf outgrows, split it.
- Trunks can be longer but should stay under ~500 lines.
- When you create a new leaf, update the trunk to point to it (so future sessions can find it).

## Cross-domain communication

Sometimes one domain discovers something another domain needs to know. For example, your Marketing session discovers that a client mentioned in the Clients domain has gone quiet.

**Simple approach (start here):**
Write a note in the other domain's Queue:

```markdown
## Cross-Domain Note (from Marketing, 2026-04-10)
Client [name] hasn't engaged with any content in 3 weeks.
May need a check-in. See MARKETING_LOG.md 2026-04-10 for details.
```

**Structured approach (scale to this):**
As your system grows, you might want a shared knowledge table. The Oxide case study uses a database table called Cortex where any domain can write findings and any other domain can read them at startup. This is advanced - you'll know when you need it.

**Add to CLAUDE.md when ready:**

```markdown
## Cross-Domain Updates
Changes spanning multiple domains: apply to ALL affected domains.
Never assume one domain will "just know" what happened in another.
```

## Scaling CLAUDE.md

Your CLAUDE.md will grow as your system grows. Keep it organised:

```markdown
# CLAUDE.md

## Who I Am
[Brief personal context]

## Core Rules
[5-10 universal rules that apply to every session]

## Domain Isolation
[Table mapping domains to their files]

## File Structure
[The 3-file pattern explanation]

## Session Rules
[Start/end protocols]

## Command Routing
[If topic is about X, suggest /X command]

## Key Paths
[Important directories and files]
```

**What goes in CLAUDE.md vs domain files:**
- **CLAUDE.md** = rules that apply to ALL domains (session discipline, file patterns, isolation)
- **Domain files** = context specific to one domain (business rules, tool configs, processes)

Don't put domain-specific rules in CLAUDE.md. It should be lean enough that loading it doesn't eat significant context.

---

# Phase 5: Automation and Infrastructure

## Git for versioning your system

Your markdown files are your system's state. Git gives you:
- **Version history** - see what changed and when
- **Backup** - push to a remote and your system survives a dead laptop
- **Sync** - work from multiple machines
- **Rollback** - undo mistakes

If you ran `git init` in Phase 1, you're already set up. Start committing:

```bash
git add CLAUDE.md BUSINESS_QUEUE.md BUSINESS.md
git commit -m "initial system setup"
```

**Commit discipline:**
- Commit after each session (your wrap-up protocol should include this)
- Use descriptive messages: `business: added client onboarding workflow`
- Push to a remote (GitHub, GitLab) for backup

**Add to your session end protocol in CLAUDE.md:**

```markdown
## Session End Protocol
When I say "wrap up":
1. Update QUEUE
2. Write HANDOFF
3. Git commit and push
```

**Important:** If your system contains sensitive information (API keys, client data, credentials), either:
- Use a **private** repository
- Add sensitive files to `.gitignore`
- Use `.claude/settings.local.json` (git-ignored by default) for secrets

## Server setup (optional but useful)

Once your system works locally, you might want:
- A server that runs n8n 24/7
- A central place your git repo lives
- Scheduled automations that run without your laptop open

**Common setup:**
- A VPS (DigitalOcean, Hetzner, AWS Lightsail) running Linux
- Docker for containerised services (n8n, databases, etc.)
- Git repo cloned on the server
- File syncing (Syncthing, rsync) between your machine and the server

**This is optional.** Many automation systems work perfectly fine running locally. Only add a server when you have workflows that need to run when your computer is off - scheduled emails, monitoring, background processing.

**If you do set up a server, document it:**

```markdown
# INFRASTRUCTURE.md

## Server
- Provider: [e.g., Hetzner, DigitalOcean]
- OS: Ubuntu 24.04
- SSH: ssh user@your-ip

## Services (Docker)
| Service | Port | Purpose |
|---------|------|---------|
| n8n | 5678 | Workflow automation |
| [database] | 5432 | Data storage |

## Deployment
How to push changes from local to server:
[your deployment steps]
```

## Syncing across devices

If you work from multiple machines:

**Option 1: Git only**
- Push from one machine, pull from the other
- Simple but manual
- Works for files that don't change in real-time

**Option 2: File sync (Syncthing, etc.)**
- Real-time bidirectional sync
- Changes appear on all machines within seconds
- Better for frequently-changing files like queues and handoffs

**Option 3: Both**
- Syncthing for real-time working files
- Git for version history and backup
- This is what Oxide uses

## Monitoring and health checks

As your system grows, things can break silently. Add simple monitoring:

**In CLAUDE.md:**

```markdown
## Health Checks
- If something "should be working" but isn't: check, don't assume.
- After building or modifying any workflow: "needs testing to confirm."
- Never assume it works. If untested, say so.
```

**With n8n:**
- Build a simple health check workflow that runs daily
- Have it verify your critical automations are still active
- Send yourself a notification if something's down

**The principle:** Build oversight into the system. If a process can fail silently, add a check. You'd rather get a "something broke" alert than discover it three weeks later.

## Scheduled automation

n8n is your primary tool for scheduling. Common patterns:

| Schedule | What | How |
|----------|------|-----|
| Daily morning | Task summary notification | n8n cron -> read queue files -> send Telegram/email |
| Weekly | Progress review | n8n cron -> compile log entries -> generate report |
| On trigger | New lead notification | n8n webhook -> CRM update -> notification |
| Hourly | Health check | n8n cron -> check services -> alert if down |

**Start with one scheduled workflow.** A daily task summary is a good first automation - it reinforces the system by surfacing your queue every morning.

---

# Reference

## Claude Code keyboard shortcuts

| Shortcut | What It Does |
|----------|-------------|
| `/` | Open slash command menu |
| `Escape` | Cancel current generation |
| `Ctrl+C` | Exit Claude Code |

**Plan Mode:** When you need to plan before executing, ask Claude to enter plan mode. It will outline steps before taking action - useful for complex multi-step tasks.

## Useful CLAUDE.md patterns

### Command routing table

When your system has multiple domains, help Claude suggest the right one:

```markdown
## Command Routing
If the topic is about...     -> Suggest
Client work, proposals       -> /clients
Content, social media        -> /marketing
Invoices, tax, bookkeeping   -> /finances
Server, Docker, deployment   -> /infrastructure
```

### Output to files

For long content, have Claude write to files instead of dumping text in the conversation:

```markdown
## Output Rules
Long content (>20 lines) -> save to ~/my-system/outputs/ with a dated filename.
```

### Progress tracking

For complex sessions:

```markdown
## Progress Tracking
After completing any task, update the QUEUE immediately.
If working on something complex, save progress to {DOMAIN}_PROGRESS.md
as you go. Don't wait for a natural pause. Context can compress without warning.
```

## Common n8n + Claude Code patterns

### Pattern 1: Workflow inspection
```text
You: "Show me what the daily report workflow does"
Claude: [fetches workflow via n8n MCP, explains each node]
```

### Pattern 2: Workflow debugging
```text
You: "The client notification workflow failed last night"
Claude: [checks execution history, reads error logs, identifies the broken node]
```

### Pattern 3: New workflow
```text
You: "Build a workflow that checks my email every hour and
      adds new client inquiries to my CRM"
Claude: [creates workflow with email trigger -> parser -> CRM API node]
```

### Pattern 4: Workflow from queue
```text
You: "/business" (loads queue showing "automate follow-up sequence")
Claude: [reads task, builds n8n workflow, marks task complete in queue]
```

## Troubleshooting

### "Claude doesn't remember what we discussed yesterday"

That's by design. Check your Handoff file - did you write one? If not, start doing that. The Handoff IS the memory. If this keeps happening, add "write the Handoff" explicitly to your session end protocol in CLAUDE.md so Claude can't close a session without doing it.

### "Claude is loading too much context"

Check your slash command - is it reading files it doesn't need? Tighten the loading order. Only read what the current task requires.

### "My queue file is a mess"

Time to split domains (Phase 4) or clean up:
- Remove completed items (move to Log)
- Group remaining tasks logically
- Update the Quick Resume section

### "Claude keeps doing things I don't want"

Add a rule to CLAUDE.md. Every correction is a future rule:
```markdown
# Bad: hoping Claude remembers
"Don't do that again"

# Good: writing it down
## Rules
- Never modify config files without asking first.
```

This is actually the best use of CLAUDE.md - it's a living record of what Claude needs to be told.

### "I don't know what to work on next"

Create a `NEXT_ACTIONS.md` - a priority-ordered list across all domains. Max 10 items. This is your "what should I do?" file.

```markdown
# Next Actions (Priority Order)

1. [ ] /clients - Send proposal to [client name] (deadline: Friday)
2. [ ] /marketing - Write this week's content post
3. [ ] /finances - Reconcile March invoices
4. [ ] /clients - Follow up with [lead name]
5. [ ] /marketing - Set up email automation
```

## The growth path

Here's how this system typically evolves:

```
Week 1:   CLAUDE.md + 1 domain (Queue, Reference, Handoff)
Week 2:   Slash command + n8n MCP connected
Month 1:  2-3 domains + domain isolation rules
Month 2:  Agents for delegation + hooks for automation
Month 3:  Server setup + scheduled workflows
Month 6:  5+ domains + cross-domain communication
Year 1:   Full system running your business and life
```

You don't need to plan all of this. Build what you need now and expand when the current setup isn't enough. The architecture scales because each domain is independent - adding domain #5 doesn't make domains 1-4 more complex.

**The most important thing:** Start using it. A system you use daily beats a perfect system you're still designing. Your CLAUDE.md will be messy. Your queue will have formatting you'll change later. That's fine - the system improves by being used, not by being planned.

---

> *"CLAUDE.md is the soil. Every domain is a tree that grows from it. The trunk is the domain's main reference doc, the branches are its supporting files, the leaves are the specialised sub-docs. Trees don't compete with each other, they share the soil. You don't plant the whole forest at once. You plant one tree, let it take root, then plant the next. The forest emerges."*

---

*Built with Claude Code. This guide is a living document - update it as Claude Code evolves and as you discover what works for your system.*
