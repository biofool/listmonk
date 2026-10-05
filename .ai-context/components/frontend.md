# Component: frontend

## Responsibility

Admin SPA (`frontend/`, Vue 3 + Buefy, Vite) and the visual e-mail editor
(`frontend/email-builder/`, React, built as a library consumed by the SPA).

## Files

- `frontend/src/` — SPA
- `frontend/email-builder/` — React editor (hardcoded English strings;
  upstream i18n gap tracked in knadh/listmonk#3246, fixed by PR #3247)
- `frontend/cypress/` — e2e suite

## Rules

- Yarn, not npm (`yarn.lock` committed — never gitignore lockfiles).
- i18n strings belong in `i18n/*.json`, not hardcoded in components.
