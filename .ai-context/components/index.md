# Components Index

| File | Component | Traffic |
|------|-----------|---------|
| `queries-layer.md` | `queries/*.sql` → `models.Queries` → `internal/core` | Highest — every DB read/write |
| `optin-mail.md` | opt-in hook, unsub URL chain, RFC 8058 headers | High — public, deliverability |
| `frontend.md` | Vue admin SPA + React email-builder | Medium |
| `ai-config-layer.md` | fork-local AI config (rules/skills/analysis) | Local only |
