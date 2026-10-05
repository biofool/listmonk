# System Overview — biofool/listmonk (fork of knadh/listmonk)

## A. System Context

```
public subscriber pages (static/, cmd/public.go)
   POST /subscription/:campUUID/:subUUID   (unsub / prefs / one-click)
   GET  /subscription/:campUUID/:subUUID?manage=true
        │
admin SPA (frontend/, Vue) ── /api/* ──► cmd/*.go handlers
        │                                │  auth (internal/auth, authz)
        │                                ▼
        │                          internal/core/*  (business logic)
        │                                │ models.Queries (sqlx)
        │                                ▼
        │                          PostgreSQL ─ schema.sql
        │                            ├─ lists, subscribers, subscriber_lists
        │                            ├─ campaigns, campaign_lists
        │                            └─ mat_list_subscriber_stats (matview:
        │                               per-list per-status counts; refreshed
        │                               by core via refreshCache)
        │
internal/manager ── campaign send queue, builds messages
   internal/manager/message.go — UnsubURL = {root}/subscription/{campUUID}/{subUUID}
   internal/manager/manager.go — sets List-Unsubscribe / List-Unsubscribe-Post
        │
cmd/subscribers.go makeOptinNotifyHook — double opt-in confirmation mail;
   uses placeholder campaign UUID (dummyUUID, all-zeros) in UnsubURL since
   no campaign exists. Headers deliberately present for deliverability.
        │
SMTP (internal/messenger, knadh/smtppool)
```

### Trust boundaries

- **Public surface** (`cmd/public.go`): unauthenticated handlers driven by
  UUIDs in path params. Anything reached via an e-mail URL is adversarial
  input — placeholder UUIDs land here too.
- **Admin surface** (`cmd/*.go`): session + permission-gated
  (`auth.GetPermittedLists` etc.).
- **DB**: all SQL lives in `queries/*.sql`; no string-built SQL in Go
  except `makeSearchQuery` order/where templating.
- **Mail**: content via Go html templates in `static/email-templates/` +
  `i18n/*.json`; headers assembled in `internal/manager` / opt-in hook.

## B. Deployable Units

Single Go binary (`main.go`) serving admin API+SPA and public pages;
PostgreSQL required; SMTP required for sends. Docker image via upstream
Dockerfile. No background workers beyond in-process manager/bounce loops.

## C. Components

See `../components/` — `queries-layer.md` (SQL→struct wiring),
`optin-mail.md` (opt-in hook + unsub URL chain), `frontend.md`,
`ai-config-layer.md` (this fork's biofool config).

## D. Runtime/Code Paths

1. **List read**: `GET /api/lists{/,:id}` → `QueryLists`/`GetList` →
   `query-lists` SQL → `statuses` CTE sums `mat_list_subscriber_stats`.
   `get-lists` (minimal mode) returns no counts.
2. **Opt-in confirm**: subscribe → `makeOptinNotifyHook` → opt-in mail
   (confirm link + unsub URL with dummyUUID).
3. **One-click unsub**: MUA POST → `SubscriptionPrefs` →
   `UnsubscribeByCampaign` (real campUUID) or `UnsubscribeUnconfirmed`
   (dummyUUID) → `subscriber_lists` status update.
4. **Campaign send**: manager dequeues → builds message w/ real campUUID →
   messenger → SMTP.

## E. Change Impact Summary

| Change | Affects | Risk |
|--------|---------|------|
| `queries/*.sql` | `models.Queries` wiring, all callers | Medium — name/tag must match |
| `schema.sql` | New installs + migrations | High |
| `mat_list_subscriber_stats` shape | Every count read | High — silent wrong numbers |
| Opt-in/unsub handlers | Deliverability + RFC 8058 compliance | High — public, unauthenticated |
| `AGENTS.md`/`.devin/`/`.ai-context/` | Local master only | Low — must never reach upstream PRs |
