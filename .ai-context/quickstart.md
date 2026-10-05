# Quickstart — biofool/listmonk

## Identity

Fork of `knadh/listmonk`. Remotes: `origin` = `biofool/listmonk` (push),
`upstream` = `knadh/listmonk` (fetch only for branching).

## Golden rules (from AGENTS.md)

1. Upstream PR branches come from `upstream/master`, never local `master`.
2. biofool is the maintainer of record; Devin is never mentioned in
   commits, PRs, comments, or docs.
3. AI config (`AGENTS.md`, `CLAUDE.md`, `.devin/skills/`, `.ai-context/`)
   commits independently on local `master` — standalone commit, never
   inside a fix branch. Update cycle: remove path from `.git/info/exclude`
   → commit → re-add.
4. Go: `gofmt` everything; `go build ./...` + `go vet` are the gate
   (upstream has no Go test suite; Cypress exists for the admin UI).

## Build / verify

```bash
go build ./...          # backend
go vet ./...
cd frontend && yarn install && yarn build   # admin SPA + email-builder
```

Runtime needs PostgreSQL + `config.toml` (`listmonk --install` scaffolds).
Local dev docs: upstream `docs/docs/content/`.

## Where things live

- Add/change a SQL query → `queries/*.sql` (`-- name:` block) + field in
  `models/queries.go` (name must match the `query:"…"` tag).
- HTTP handler → `cmd/<area>.go` (admin API) or `cmd/public.go` (public).
- Business logic → `internal/core/<area>.go`.
