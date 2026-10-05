# Workflow: upstream PR (biofool fork → knadh/listmonk)

1. `git fetch upstream && git checkout -b fix/<name> upstream/master`
   — never branch from local `master` (it carries the AI config layer).
2. Make the minimal fix; `gofmt`; `go build ./...` + `go vet` (no Go tests
   upstream; Cypress for UI work).
3. Commit with a message that names the upstream issue — no Devin
   attribution of any kind (see AGENTS.md §Attribution).
4. `git push -u origin fix/<name>`; `gh pr create --repo knadh/listmonk
   --head biofool:fix/<name>`.
5. Verify the PR diff contains only fix files — AGENTS.md/.devin/
   .ai-context must not appear.
6. Optionally mirror as a local tracking issue on biofool/listmonk.
