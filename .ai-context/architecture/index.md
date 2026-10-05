# Architecture Index

## Files

- `system-overview.md` — request/data flow, trust boundaries, deployable units

## Deployable Units

One Go binary (admin API + SPA + public pages). PostgreSQL + SMTP required.
Cypress suite for admin UI; no Go test suite upstream.

## Components (see `../components/`)

| Component | Role |
|-----------|------|
| `cmd/` handlers | HTTP surface (admin + public) |
| `internal/core/` | Business logic over `models.Queries` |
| `models.Queries` | Named-SQL prepared statements (goyesql) |
| `internal/manager` | Campaign queue, List-Unsubscribe headers |
| `frontend/` | Vue admin SPA + React email-builder |
| AI config layer | `AGENTS.md`, `CLAUDE.md`, `.devin/skills/`, `.ai-context/` — local master only |
