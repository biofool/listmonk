# Architecture Decisions

## ADR-001: Fork carries biofool config on local master, PRs from upstream/master

**Decision**: `AGENTS.md`, `CLAUDE.md`, `.devin/skills/`, `.ai-context/` are
committed on local `master` (diverging from upstream) in dedicated commits;
all upstream-bound branches are cut from `upstream/master`.

**Why**: keeps the config layer discoverable by agents while guaranteeing
zero leakage into upstream PR diffs — a branch-based boundary is enforced
by git itself; `.git/info/exclude` is the belt-and-suspenders second layer.

## ADR-002: One-click unsubscribe on opt-in mail cancels pending opt-ins

**Decision**: keep the `List-Unsubscribe` header (deliverability, #2224)
and make its POST functional for the placeholder campaign UUID by marking
`unconfirmed` subscriptions `unsubscribed` (#3250/#3063, PR #3256).

**Rejected**: removing the header (breaks MSP requirements); blocklisting
the subscriber (over-reach for a pending opt-in).
