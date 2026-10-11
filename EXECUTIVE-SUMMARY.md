# Executive Summary — listmonk (biofool fork)

listmonk is a self-hosted newsletter and mailing list manager. It runs as a
single Go binary with a PostgreSQL database. This repo is the biofool fork of
[knadh/listmonk](https://github.com/knadh/listmonk).

## What it gives somatic studios

For movement studios (aikido, yoga, dance, somatic education), listmonk
replaces a hosted email marketing service with software the studio controls.

- **Own the subscriber list.** Student emails stay on the studio's own
  server. No third-party platform holds the relationship data.
- **Class and workshop announcements.** Segment the list (e.g. beginners,
  members, workshop interest) and send targeted campaigns. Campaigns support
  scheduling, templates, tracking, and attachments.
- **Transactional email.** A `/api/tx` endpoint sends templated messages
  (welcome emails, schedule changes, receipts) from other studio systems.
- **Double opt-in built in.** Public subscription forms, confirmation
  emails, one-click unsubscribe, and per-list opt-in settings come
  standard.
- **Bounce handling.** Automatic bounce processing via POP3/IMAP mailbox
  scans or webhooks from providers (SES, SendGrid, Postmark, and others)
  keeps the list clean.
- **Low operating cost.** It runs on a small VM or a Docker host. There is
  no per-subscriber or per-email fee — only the SMTP provider's cost.
- **Multi-studio or multi-program use.** One install can serve several
  lists, roles, and admin users with granular permissions.

## Status

This fork tracks upstream `knadh/listmonk` and contributes fixes back via
upstream PRs. Local `master` carries biofool-specific config and docs.
