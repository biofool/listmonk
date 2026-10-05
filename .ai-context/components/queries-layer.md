# Component: queries layer (named SQL → models.Queries)

## Responsibility

Single source of SQL. `queries/*.sql` holds `-- name: <kebab>` blocks;
`models/queries.go` mirrors each into `Queries` via `query:"<kebab>"` tags
(goyesql). `internal/core/*` calls `c.q.<Field>.Select/.Exec`.

## Files

- `queries/*.sql` (~109 named queries; subscribers.sql has 33)
- `models/queries.go` — field-per-query struct
- `internal/core/*.go` — call sites

## Rules

- New query = `-- name:` block + matching `*sqlx.Stmt` field (prepared) or
  `string` field (arbitrary/dynamic queries like `query-subscribers`).
- `query-lists` populates `subscriber_statuses` (JSONB map) AND
  `subscriber_count` (pre-summed, COALESCE→0) — do NOT re-sum in Go
  (was bug knadh/listmonk#3253).
- `get-lists` / `get-lists-by-optin` are minimal (no count columns).
