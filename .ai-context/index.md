# AI Context Index — biofool/listmonk (fork of knadh/listmonk)

> **Last analyzed**: 2026-10-05
> **Staleness check**: compare `git rev-parse upstream/master` with the
> analysis date; if materially ahead, re-run the analysis on changed paths.

## What This Repo Is

A fork of `knadh/listmonk` — self-hosted newsletter and mailing-list
manager (Go backend + Vue frontend + React email-builder). The fork exists
to carry biofool's contributions back upstream; `origin` = biofool fork,
`upstream` = knadh/listmonk. Local `master` tracks upstream plus a
biofool-only config layer (AGENTS.md, CLAUDE.md, .devin/skills, .ai-context).

**Classification**: FULL APPLICATION — deployable binary, PostgreSQL store,
SMTP, admin UI, public subscription pages.

## Repository Shape

| Area | Purpose |
|------|---------|
| `cmd/` | Entrypoint (`main.go`, `init.go`, `install.go`) + HTTP handlers (admin API `*.go`, `public.go`, `tx.go`) |
| `internal/core/` | Business logic (`lists.go`, `subscribers.go`, `campaigns.go`, …) |
| `internal/manager/` | Campaign send queue, message building, List-Unsubscribe headers |
| `internal/` (rest) | auth, authz, bounce, buflog, captcha, events, i18n, media, messenger, migrations, notifs, subimporter, tmptokens, utils |
| `models/` | DB models + `queries.go` (`Queries` struct — sqlx.Stmt fields tagged `query:"<name>"`) |
| `queries/*.sql` | Named SQL (`-- name: x`) loaded via goyesql → `models.Queries` |
| `schema.sql` | Full schema incl. `mat_list_subscriber_stats` matview |
| `frontend/` | Vue 3 admin SPA + `email-builder/` (React, built as lib) |
| `static/` | Public pages + `email-templates/` (Go templates for opt-in/notif mail) |
| `i18n/` | Language JSON |
| `.devin/skills/` | 134 AI-agent skills synced from biofool/starter |
| `.ai-context/` | This compiled analysis |

## Navigation Path

1. New to repo → `quickstart.md`
2. Understand the system → `architecture/system-overview.md`
3. Open an upstream PR → `workflows/upstream-pr.md`
4. Sync starter config → `workflows/template-sync.md`
5. Component details → `components/`

## Key Artifacts

- `AGENTS.md` — global rules + fork-specific attribution/PR-hygiene rules
- `architecture/system-overview.md` — request/data flow, trust boundaries
- `workflows/upstream-pr.md` — branch-from-upstream/master, Devin-attribution ban
- `components/queries-layer.md` — named-SQL → `models.Queries` wiring (the layer both recent fixes touched)
