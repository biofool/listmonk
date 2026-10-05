# Component: AI config layer (biofool-only, local master)

## Responsibility

biofool agent configuration living on local `master` only: `AGENTS.md`,
`CLAUDE.md` (starter global rules + fork section), `.devin/skills/`
(134 skills synced from biofool/starter), `.ai-context/` (this analysis).

## Rules

- Committed independently — standalone commit, never inside fix branches.
- `.git/info/exclude` lists all four paths; update cycle = remove from
  exclude → commit → re-add (see AGENTS.md §"AI config files").
- Upstream PR branches come from `upstream/master` so these files never
  appear in PR diffs to knadh/listmonk.
- Re-sync from `~/projects/github/starter` when its version stamp advances.
