# Workflow: AI config independent check-in

Applies to: `AGENTS.md`, `CLAUDE.md`, `.devin/skills/`, `.ai-context/`.

1. Remove the path(s) from `.git/info/exclude`.
2. `git add` the changed paths; commit on `master` as a standalone commit
   (e.g. `Sync AI config from biofool/starter`) — never mixed into a fix.
3. Re-add the path(s) to `.git/info/exclude`.

The exclude entry guards the real boundary: fix branches are cut from
`upstream/master` where these paths don't exist in the index, so the entry
keeps any stray working-tree copy invisible to `git add .`.
