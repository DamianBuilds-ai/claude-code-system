# Claude Code System

I've been using Claude Code for about nine months to manage my whole life - business operations, finances, content creation, health tracking, psychology work, side projects, and a server running 24/7 in the background. Not with a shiny app. Just markdown files, slash commands, and a system I built from scratch because every other productivity tool I tried eventually failed me in the same way: it was shaped like someone else's brain.

This repo documents how I built that system and how you can build yours.

The short version: Claude Code sessions are stateless. Every time you start a new one, the AI starts fresh. So I stopped treating Claude Code like a conversation tool and started treating it like an engine that reads and writes files. The files are the memory. The system lives in them.

---

## What this is

A 5-phase curriculum for turning Claude Code into a persistent personal operating system - one that manages your business, projects, finances, or whatever else matters to you, across sessions, without losing context.

It's not a framework you install. It's a pattern you build into your own files. By the end of it, you'll have:

- A `CLAUDE.md` that acts as your system's operating manual (loaded automatically every session)
- A domain structure where every area of work has its own queue, reference doc, and session history
- Slash commands that load the right context in seconds, not minutes
- (Optionally) connected tools, sub-agents, scheduled automations, and a server

The system I run this on is called Oxide. It has 20+ domains and has been running daily for months. This repo is the distilled, generalisable version of what I learned building it.

---

## Who this is for

You'll get the most out of this if you use Claude Code regularly and find yourself re-explaining context every session. Or if you manage several distinct areas of work (clients, finances, content, a side project) and want unified AI assistance across them. Or if you've ever thought "I wish Claude just already knew this."

You need to be comfortable in a terminal and willing to maintain markdown files. That's the actual requirement - no specific background, no framework knowledge.

---

## The 5 phases

| Phase | What you build | When |
|-------|---------------|------|
| **1. Foundation** | CLAUDE.md, core rules, the file-as-memory shift | Day 1 |
| **2. First Domain** | The 3-file system (Queue/Reference/Log), a slash command, handoffs | Week 1 |
| **3. Tools and Connections** | MCP servers, n8n, sub-agents, hooks, memory | Week 2 |
| **4. Multi-Domain** | Domain isolation, the forest architecture, cross-domain communication | Month 1 |
| **5. Automation** | Git versioning, server setup, file syncing, monitoring, scheduling | Month 2+ |

Each phase builds on the previous one. The concepts compound, so don't skip ahead.

---

## Quick start

```bash
# Clone the repo
git clone https://github.com/DamianBuilds-ai/claude-code-system.git

# Open setup.md - it's a slash command you paste into Claude Code
# It audits what you already have and maps it to the 5 phases
cat setup.md

# Or from Claude Code in your project directory:
# /setup
```

The `/setup` command doesn't replace what you have. It reads your existing Claude Code setup, figures out where you are in the 5 phases, and recommends specific additions. It builds on what's already working.

---

## What's in this repo

| File | What it is |
|------|-----------|
| `setup.md` | The coaching slash command - run this first |
| `SYSTEM_GUIDE.md` | Full 5-phase curriculum with examples and troubleshooting |
| `templates/CLAUDE.md.template` | Starting point for your system's operating manual |
| `templates/DOMAIN_QUEUE.md.template` | Active task queue for any domain |
| `templates/DOMAIN.md.template` | Reference/knowledge base for any domain |
| `templates/DOMAIN_LOG.md.template` | Session history for any domain |
| `templates/DOMAIN_HANDOFF.md.template` | Session relay baton for any domain |
| `examples/example-domain-walkthrough.md` | Full worked example: building a Clients domain from scratch |

---

## The core ideas (quick version)

**Files are memory.** Claude Code sessions are stateless - close a session and the AI forgets everything. Your files don't. Build the system in files, use Claude as the engine that processes them.

**The 3-file pattern.** Every domain (Clients, Finances, Marketing, whatever yours are) has three files: a Queue for active work, a Reference for knowledge, and a Log for history. A Handoff file bridges sessions.

**The forest.** CLAUDE.md is the soil - always present, always loaded, behavioral rules for every session. Each domain is a tree: a trunk (reference doc), branches (queue, log, handoff), and leaves (specialised sub-docs when a topic outgrows the trunk). Trees stay small so no session wastes context loading irrelevant information.

**Domain isolation.** When working on Clients, load only Clients files. When working on Finances, load only Finances files. This discipline keeps the system fast and the AI focused.

---

## Contributing

PRs welcome. If you've found patterns that work better than what's here, open one. If something in the guide is wrong or outdated, open an issue.

---

## Credit and links

Built by **DamianBuilds** - that's me. The system this is based on (Oxide) is private (it contains personal data), but I document what I learn building it publicly.

- Portfolio: [portfolio.damianbuilds.com](https://portfolio.damianbuilds.com)
- Building in public: [damianbuilds.com](https://damianbuilds.com)
- LinkedIn: [linkedin.com/in/damian-yazbeck](https://linkedin.com/in/damian-yazbeck)

---

## If you add code to this repo

This is currently a documentation-only repo. If you ever add runnable code (Python, Node, Docker, secrets), create a `.gitignore` at the root BEFORE that first commit:

```text
# Python
__pycache__/
*.pyc
*.pyo
venv/
.venv/
.pytest_cache/

# Node
node_modules/
dist/
*.log

# Env & secrets (NEVER commit these)
.env
.env.*
!.env.example
credentials.json
token.json
*.key
*.pem

# OS & editor
.DS_Store
.vscode/
.idea/
*.swp
```

If you're adding code and there's no `.gitignore`, stop and create one first.

---

## License

MIT. See `LICENSE`.
