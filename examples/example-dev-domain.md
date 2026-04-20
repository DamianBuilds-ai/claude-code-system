# Example: Mature Dev Domain (Atlas v6)

A worked example of a dev-type domain after several months of use. Files shown as snippets - not full content.

---

## DEV.md (trunk, ~180 lines)

```markdown
# Dev - Reference

> **Domain type:** dev

## Overview
Freelance development work. Covers client projects, personal tools, and infrastructure scripts.
Active projects: auth-refactor (BetaCo), portfolio-site (personal).

## Context routing

| When the user asks about...  | Load this leaf |
|------------------------------|----------------|
| Auth, tokens, OAuth          | DEV-AUTH.md    |
| Database schema, migrations  | DEV-DATABASE.md|
| Deployment, CI/CD, Docker    | DEV-INFRA.md   |
| Everything else              | (stay in trunk)|

## Tools and Services
| Tool | Purpose | Access |
|------|---------|--------|
| GitHub | Repos | github.com/[username] |
| Railway | Deploys | railway.app |
| Supabase | DB (BetaCo project) | dashboard.supabase.com |

## Rules
- Never push directly to main - always branch + PR
- BetaCo: staging deploy before prod, always
- Personal projects: deploy when it works, document when it ships

## Leaves
| File | Contents | Load when... |
|------|---------|--------------|
| DEV-AUTH.md | OAuth2 flow, token refresh, session logic | auth work |
| DEV-DATABASE.md | Schema, migrations, Supabase setup | DB work |
| DEV-INFRA.md | Railway config, Docker, CI scripts | deploy work |
```

---

## DEV_QUEUE.md (trunk, ~140 lines)

```markdown
# Dev - Queue

## Quick Resume
Auth-refactor for BetaCo is 80% done. Token refresh logic is working. Next: write tests
for the refresh endpoint, then deploy to staging. Portfolio site on hold.

## Active Work - This Session
- [ ] Write tests for /auth/refresh endpoint
- [ ] Deploy auth-refactor branch to BetaCo staging
- [ ] Verify token expiry edge case (see DEV-AUTH.md issue log)

## QUEUE Leaves
| Leaf | What lives there | Load when... |
|------|-----------------|--------------|
| DEV_QUEUE-BACKLOG.md | 8 P1 items for post-auth-refactor | Planning session |
| DEV_QUEUE-auth-refactor.md | Full BetaCo auth project tasks | Working on BetaCo |

## Recently Completed
- 2026-04-19: Implemented token refresh endpoint
- 2026-04-18: Migrated BetaCo DB to Supabase
- 2026-04-16: Set up Railway staging environment
```

---

## DEV_QUEUE-BACKLOG.md (snippet)

```markdown
# Dev - Queue Backlog

> P1 items, future work, picked up during planning sessions. Load on-demand only.

## P1 - High Priority
- [ ] Portfolio site: update case studies with BetaCo project (after launch)
- [ ] Set up automated DB backups for BetaCo Supabase
- [ ] Extract DEV-DATABASE.md - trunk section is at 85 lines, over threshold

## P2 - Do Soon
- [ ] Add error tracking (Sentry) to BetaCo app
- [ ] Write deploy runbook for Railway
```

---

## DEV_QUEUE-auth-refactor.md (project-scoped leaf, snippet)

```markdown
# Dev - BetaCo Auth Refactor

> Project-scoped QUEUE leaf. Loaded when working on BetaCo auth project.
> Archive this file to DEV_LOG_auth-refactor.md when the project closes.

## Project State
Started: 2026-04-10. Target: 2026-04-25 (BetaCo deadline).

## Task List
- [x] Audit current session handling
- [x] Design token refresh flow
- [x] Implement /auth/refresh endpoint
- [ ] Write tests for /auth/refresh
- [ ] Staging deploy + BetaCo review
- [ ] Prod deploy

## Decisions Log
- 2026-04-12: Using JWT with 15min access / 7d refresh. BetaCo approved.
- 2026-04-14: Refresh tokens stored in httpOnly cookies, not localStorage.
```

---

## DEV_LOG_auth-refactor.md (project log, snippet)

```markdown
# Dev Log - BetaCo Auth Refactor

> Project log. Created when the project-scoped QUEUE leaf was archived.
> Archive to archive/ when this is 30+ days inactive.

## 2026-04-19
- Implemented token refresh endpoint
- Wrote integration test scaffolding (3 tests passing, 2 pending)

## 2026-04-18
- Migrated BetaCo DB to Supabase - zero downtime
- Updated connection strings in Railway env vars
```

---

## Key patterns shown

- Trunk context routing table points to 3 content leaves
- QUEUE trunk stays under 200 lines with 2 QUEUE leaves pointed in the table
- Project-scoped QUEUE leaf (`DEV_QUEUE-auth-refactor.md`) becomes a project LOG on close
- Backlog leaf loads only during planning, not at session start
