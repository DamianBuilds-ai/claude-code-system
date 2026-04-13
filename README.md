# Claude Code System

A structured methodology for building a persistent, multi-domain personal operating system with Claude Code.

---

## What Is This?

Most people use Claude Code session-by-session - ask a question, get an answer, close the window, start over. This system shows you a different approach: build Claude Code into a **persistent system** where files replace memory, slash commands load context instantly, and every session picks up exactly where the last one left off.

The result is a personal OS that manages your business, finances, projects, health, and anything else - running through structured markdown files, connected tools, and automated workflows.

---

## Who Is It For?

- Builders who use Claude Code regularly and want it to stop forgetting everything
- People managing multiple areas of work (clients, finances, content, products) who want unified AI assistance
- Technical readers comfortable with Python, Docker, and Git who have never built a structured personal AI system
- Anyone who has thought "I wish Claude just knew all this already"

You do not need a degree or formal background. You need to be comfortable with a terminal and willing to maintain markdown files.

---

## The 5 Phases at a Glance

| Phase | What You Build | Timeline |
|-------|---------------|----------|
| **1. Foundation** | Your CLAUDE.md, core rules, the file-as-memory mental shift | Day 1 |
| **2. First Domain** | The 3-file system (Queue/Reference/Log), a slash command, handoffs | Week 1 |
| **3. Tools and Connections** | MCP servers, n8n integration, sub-agents, hooks, memory | Week 2 |
| **4. Multi-Domain** | Domain isolation, the forest architecture, cross-domain communication | Month 1 |
| **5. Automation** | Git versioning, server setup, file syncing, monitoring, scheduling | Month 2+ |

Each phase builds on the last. Do not skip ahead - the concepts compound.

---

## Quick Start

```bash
# 1. Clone this repo
git clone https://github.com/damianyaz-stack/claude-code-system.git

# 2. Read the coaching script - it audits what you already have and recommends next steps
# Open setup.md in your editor or:
cat setup.md

# 3. From Claude Code (in your project directory), run:
# /setup
```

The `/setup` slash command (based on `setup.md`) reads your existing Claude Code setup, maps it to the 5 phases, and recommends specific additions - not replacements. It builds on what you already have.

---

## What You Get

| File | Purpose |
|------|---------|
| `setup.md` | Coaching script - run this from Claude Code to audit your existing setup and get phase-specific recommendations |
| `SYSTEM_GUIDE.md` | Full 5-phase curriculum with examples, patterns, and troubleshooting |
| `templates/CLAUDE.md.template` | Starting point for your system's operating manual |
| `templates/DOMAIN_QUEUE.md.template` | Active task queue for any domain |
| `templates/DOMAIN.md.template` | Reference/knowledge base for any domain |
| `templates/DOMAIN_LOG.md.template` | Session history for any domain |
| `templates/DOMAIN_HANDOFF.md.template` | Session relay baton for any domain |
| `examples/example-domain-walkthrough.md` | Full worked example: building a Clients domain from scratch |

---

## Key Concepts

**Files are memory.** Claude Code sessions are stateless - when you close a session, the AI forgets everything. But your files don't. The system is built in files; the AI is the engine that processes them.

**The 3-file pattern.** Every domain (Clients, Finances, Marketing, etc.) has three files: a Queue for active work, a Reference for knowledge, and a Log for history. A fourth Handoff file bridges sessions.

**The forest metaphor.** CLAUDE.md is the soil - always present, always loaded. Each domain is a tree: a trunk (reference doc), branches (queue, log, handoff), and leaves (specialized sub-docs). Trees are kept small so no session wastes context loading irrelevant information.

**Domain isolation.** When working on Clients, load only Clients files. When working on Finances, load only Finances files. This is the discipline that keeps the system fast and focused.

---

## Contributing

Issues and pull requests welcome. If you have patterns that work better than what's described here, open a PR with the improvement.

---

## Credit

This methodology was developed by **Damian Yazbeck** at **DamianBuilds** through a working system called Oxide - 20+ domains managing business, finances, content, health, and infrastructure through Claude Code. The patterns here are extracted, generalised, and cleaned for reuse.

**Follow along:**
- Portfolio: [portfolio.damianbuilds.com](https://portfolio.damianbuilds.com)
- LinkedIn: [linkedin.com/in/damian-yazbeck](https://linkedin.com/in/damian-yazbeck)
- Building in public: [damianbuilds.com](https://damianbuilds.com)

---

## Before Adding Code To This Repo

This is currently a pure-documentation repo. No code, no .gitignore needed.

If you ever add runnable code (Python, Node, Docker, secrets), drop a `.gitignore` at the repo root with these defaults BEFORE the first commit:

```
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

Treat this as the standing rule. If you're adding code and you don't see a .gitignore, STOP and create one first.

---

## License

MIT License. See `LICENSE`.
