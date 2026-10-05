# Component: opt-in mail & unsubscribe chain

## Responsibility

Double opt-in confirmation mail and the unsubscribe endpoints its
`List-Unsubscribe` URL hits.

## Files

- `cmd/subscribers.go` — `makeOptinNotifyHook`, `dummyUUID` placeholder
- `cmd/public.go` — `SubscriptionPrefs` (GET prefs page / POST one-click)
- `internal/core/subscribers.go` — `UnsubscribeByCampaign`,
  `UnsubscribeUnconfirmed`
- `queries/subscribers.sql` — `unsubscribe-by-campaign`,
  `unsubscribe-unconfirmed-subscriptions`
- `internal/manager/message.go`, `manager.go` — real campaign UUID +
  headers on campaign mail
- `static/email-templates/subscriber-optin.html` — confirm + `?manage=true`
  footer link

## Rules / history

- `List-Unsubscribe` on opt-in mail is deliberate (knadh/listmonk#2224,
  commit e8fd12b) — deliverability. Do not remove.
- `dummyUUID` = `00000000-0000-0000-0000-000000000000`; only opt-in mail
  uses it in the unsub URL. POSTs carrying it cancel `unconfirmed` rows
  (fix for knadh/listmonk#3250/#3063); blocklist POSTs still use
  `unsubscribe-by-campaign` (matches by subscriber UUID).
