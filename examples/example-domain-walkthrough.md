# Building a Clients Domain from Scratch

This is what it actually looks like to set up a domain from zero - files, slash command, first session, the disciplines that make it stick. I'm using "Clients" because it's the most common starting point for freelancers and consultants, but the pattern is identical for any domain you build.

Set aside about 30 minutes for the initial setup. After that, it mostly runs itself.

---

## Step 1: Define the domain before touching any files

Two questions before you create anything:

**What does this domain actually cover?** Be specific about where it starts and stops. For Clients: tracking leads, sending proposals, managing active projects, following up. NOT invoicing (that's Finances) and NOT social media outreach to attract clients (that's Marketing). Clear edges matter because the AI will stay inside them.

**What's the single most common task you'll do here?** For Clients, it's probably drafting and sending proposals. Design the domain around that task first.

---

## Step 2: Create the files

```bash
cd ~/my-system

touch CLIENTS_QUEUE.md
touch CLIENTS.md
touch CLIENTS_LOG.md
touch CLIENTS_HANDOFF.md
```bash

Fill in the Queue first. Everything else comes after.

---

## Step 3: Fill in the Queue

Your first Queue will look something like this. Replace the example content with your actual situation:

```markdown
# Clients - Queue and Active Work

## Quick Resume
Two active clients, one proposal outstanding. Priority this week:
follow up with [lead name] who went quiet after the call on Monday.
Next: check in, then draft proposal for the new inquiry from [name].

---

## Queue

### Active Projects
- [ ] [Client A] - deliver revised mockups by Friday
- [ ] [Client B] - weekly check-in call Thursday 2pm

### Proposals Out
- [ ] Follow up with [Lead X] - no reply since call on 2026-04-07
- [ ] Draft proposal for [Lead Y] - new inquiry, details in email

### Leads to Nurture
- [ ] Research [Company Z] before reaching out
- [ ] Reply to referral from [contact name]

---

## Recently Completed
- 2026-04-08: Sent proposal to [Lead X]
- 2026-04-07: Discovery call with [Lead X] - went well
```markdown

The Queue is now the source of truth for this domain. Claude reads it first every session. You update it every session. It's the one file that has to stay current.

---

## Step 4: Fill in the Reference doc

The Reference doc holds stable background knowledge - things that don't change session to session.

```markdown
# Clients - Reference

## Overview
Freelance [your service type] business. Clients are typically [description of ideal
client: company size, industry, problem they have]. Average project value: [range].
Average project length: [range].

## Tools and Services
| Tool | Purpose | Access |
|------|---------|--------|
| [CRM name] | Lead and client tracking | [URL] |
| Stripe | Invoicing and payment | dashboard.stripe.com |
| [Calendar tool] | Scheduling discovery calls | [URL] |
| [Project tool] | Active project management | [URL] |

## Processes

### Proposal Process
1. Discovery call completed and notes saved
2. Proposal drafted using template at ~/my-system/templates/proposal.md
3. Proposal sent via [platform/email]
4. Follow up after 3 business days if no reply
5. On acceptance: send contract, collect deposit, kick off project

### Client Onboarding (Post-Proposal)
1. Contract signed via [tool]
2. 50% deposit collected
3. Project kickoff doc sent (template at ~/my-system/templates/kickoff.md)
4. Slack channel / workspace created
5. First milestone confirmed and scheduled

### Project Close
1. Final deliverable delivered and approved
2. Invoice for remaining balance sent
3. Testimonial request sent (2 weeks after delivery)
4. Case study notes saved to CLIENTS_LOG.md

## Pricing
- [Service type A]: $[range] depending on scope
- [Service type B]: $[rate]/hr
- Rush fee: 25% surcharge for <1 week turnaround

## Rules
- Never start work without a signed contract and deposit
- All client communication tracked in [CRM name]
- Proposals valid for 14 days
```markdown

---

## Step 5: Create the slash command

Create `.claude/commands/clients.md` in your project:

```bash
mkdir -p .claude/commands
```

```markdown
You are helping me manage my client relationships and business development.

## Loading Order
1. Read `CLIENTS_QUEUE.md` first - this is the active work queue.
2. Read `CLIENTS_HANDOFF.md` - what happened last session.
3. Read `CLIENTS.md` only if you need deeper context (pricing, processes, tools).

## Rules
- Update the QUEUE after completing any task.
- Write a HANDOFF entry at session end.
- One task at a time. Finish it before starting the next.
- Save progress constantly. Don't rely on conversation memory.
- Never start work on a project without a signed contract. If a client pushes back,
  flag it - don't just proceed.

## What You Help With
- Drafting proposals and following up on them
- Managing active project timelines and check-ins
- Lead research and qualification
- Onboarding new clients
- Closing out completed projects

ARGUMENTS: $ARGUMENTS
```markdown

Type `/clients` in Claude Code to start a session with full context loaded automatically.

---

## Step 6: Run your first session

Open Claude Code in your project directory and type:

```text
/clients
```

Claude will read your Queue and Handoff (empty for now), then report back on what's active and what's next.

A first session might look like this:

```text
You: /clients

Claude: [reads Queue, sees 2 active clients and 2 outstanding proposals]
        Active: [Client A] mockups due Friday, [Client B] call Thursday.
        Priority: follow up with [Lead X] who hasn't replied since Monday.
        Want me to draft a follow-up message?

You: Yes, follow up with Lead X. Keep it short, just checking in.

Claude: [drafts message]

You: Good. Mark that task done and wrap up.

Claude: [updates Queue - checks off "follow up with Lead X"]
        [writes Handoff with what was done]
        Done. Follow-up drafted and Queue updated.
```text

That's one session. The next time you type `/clients`, Claude reads the Handoff and knows exactly what happened.

---

## Step 7: The habits that make it actually work

After two weeks of using this pattern, a few disciplines make the difference between a system that holds and one that quietly falls apart:

**Always start with the slash command.** `/clients` instead of "hey help me with a client thing." Without it, Claude starts cold and you spend the first five minutes re-explaining context you've already written down.

**Update the Queue before ending the session.** Skip this once and the next session starts with stale information. The rule is simple: don't close the tab until Queue and Handoff are updated.

**Keep the Quick Resume current.** The 2-3 sentences at the top of the Queue are what the next session sees first. If the situation changed this session, those sentences need to change too.

**Use the Log for rotation, not deletion.** When "Recently Completed" has 5+ items, move the oldest to the Log. Don't delete them - the history is useful, and you'll be surprised how often you want to look back.

---

## Step 8: Adding a leaf when the domain grows

After a few months, you might find the Reference doc is getting long. Suppose your proposal templates, counter-argument scripts, and pricing tiers have grown to 200 lines. Extract them:

```bash
touch CLIENTS_PROPOSALS.md
```

Move the proposal-related content there. Update `CLIENTS.md` to point to it:

```markdown
## Sub-Documents (Leaves)

| File | Contents |
|------|---------|
| CLIENTS_PROPOSALS.md | Proposal templates, pricing tiers, objection handling |
```markdown

Update the slash command to mention the leaf:

```markdown
## Loading Order
1. Read `CLIENTS_QUEUE.md` first.
2. Read `CLIENTS_HANDOFF.md`.
3. Read `CLIENTS.md` only if you need deeper context.
4. Read `CLIENTS_PROPOSALS.md` only when drafting or reviewing proposals.
```markdown

The leaf loads on demand. Sessions that don't touch proposals never load it - context is preserved.

---

## What this looks like after 3 months

```text
CLIENTS.md                     (stable: processes, pricing, tools)
CLIENTS_QUEUE.md               (changes every session: tasks, quick resume)
CLIENTS_LOG.md                 (grows over time: completed work history)
CLIENTS_HANDOFF.md             (updated each session: relay baton)
CLIENTS_PROPOSALS.md           (leaf: grows when you add new templates)
.claude/commands/clients.md    (slash command: evolves as the domain does)
```text

Six files, one domain, full persistent AI assistance. Every session picks up exactly where the last one left off. Claude knows your processes, your clients, your rules, and what's currently in flight - because it reads the files, not your memory.

---

## When things go wrong

**"Claude keeps asking me to clarify basic things about my business."**
Add them to the Reference doc. Every question Claude asks that it should already know is a signal that something belongs in `CLIENTS.md`.

**"The Queue is getting too long."**
Split into two domains (e.g., `ACTIVE_PROJECTS` and `BUSINESS_DEVELOPMENT`), or clean up by moving completed items to the Log and dropping tasks you're no longer pursuing.

**"I forgot to write the Handoff for the last session."**
Write it now from memory. A late Handoff beats no Handoff. Don't skip it going forward.

**"I have a client situation that's too sensitive to put in files."**
Use a private git repo. Or keep the sensitive fields vague - code names, initials, whatever level of detail you're comfortable committing to disk. The system works with whatever you give it.
