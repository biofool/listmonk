# Workflow: template sync (starter → this repo)

1. Read `~/projects/github/starter/AGENTS.md` (or fetch from
   github.com/biofool/starter) — compare its version stamp to ours.
2. Replace the global rules block in `AGENTS.md`; preserve the
   "Project-specific: listmonk" section verbatim.
3. Mirror into `CLAUDE.md`; bump both version stamps to sync date.
4. Re-copy `.devin/skills/` wholesale (`cp -r starter/.devin/skills .devin/`).
5. Refresh `.ai-context/` if repo shape changed.
6. Commit per `ai-config-checkin.md`.
