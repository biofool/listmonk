# Security — listmonk

Fork of knadh/listmonk. 2026-10 audit: `recordMigrationVersion` SQL parameterized — upstream PR https://github.com/knadh/listmonk/pull/3270 ; fork vite `LISTMONK_` env-prefix fix (moot after upstream SSR refactor removed frontend/).

## Standing rules
- Never commit secrets (`.env`, `*.key`, `credentials*.json` are gitignored).
- Never read or log secret values — key names and lengths only.
- Security fixes land reviewed-but-uncommitted first, then roll dev → main.
