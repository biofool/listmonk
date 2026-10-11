<a href="https://zerodha.tech"><img src="https://zerodha.tech/static/images/github-badge.svg" align="right" /></a>

[![listmonk-logo](https://user-images.githubusercontent.com/547147/231084896-835dba66-2dfe-497c-ba0f-787564c0819e.png)](https://listmonk.app)

listmonk is a standalone, self-hosted, newsletter and mailing list manager. It is fast, feature-rich, and packed into a single binary. It uses a PostgreSQL database as its data store.

> **This repo is the biofool fork of [knadh/listmonk](https://github.com/knadh/listmonk).**
> It tracks `upstream/master` and contributes fixes back via upstream PRs.
> See `EXECUTIVE-SUMMARY.md`, `TECHNOLOGY-UNDERPINNINGS.md`, and the `wiki/` folder for fork-local docs.

[![listmonk-dashboard](https://github.com/user-attachments/assets/689b5fbb-dd25-4956-a36f-e3226a65f9c4)](https://listmonk.app)

Visit [listmonk.app](https://listmonk.app) for more info. Check out the [**live demo**](https://demo.listmonk.app).

## Installation

### Docker

The latest image is available on DockerHub at [`listmonk/listmonk:latest`](https://hub.docker.com/r/listmonk/listmonk/tags?page=1&ordering=last_updated&name=latest).
Download and use the sample [docker-compose.yml](https://github.com/knadh/listmonk/blob/master/docker-compose.yml).


```shell
# Download the compose file to the current directory.
curl -LO https://github.com/knadh/listmonk/raw/master/docker-compose.yml

# Run the services in the background.
docker compose up -d
```
Visit `http://localhost:9000`

See [installation docs](https://listmonk.app/docs/installation)

__________________

### Binary
- Download the [latest release](https://github.com/knadh/listmonk/releases) and extract the listmonk binary.
- `./listmonk --new-config` to generate config.toml. Edit it.
- `./listmonk --install` to setup the Postgres DB (or `--upgrade` to upgrade an existing DB. Upgrades are idempotent and running them multiple times have no side effects).
- Run `./listmonk` and visit `http://localhost:9000`

See [installation docs](https://listmonk.app/docs/installation)
__________________

## Using this repo (development)

Prerequisites: Go (see `.go-version`, currently 1.26.x), [Bun](https://bun.sh) for the admin frontend build, Python 3 for the icon sprite script, PostgreSQL.

```bash
make run              # build frontend + run backend in dev mode (DEV_CONFIG points at dev config)
make build            # compile the Go backend to ./listmonk
make build-frontend   # build the SSR admin frontend (Bun) into static/admin/dist
make test             # go test ./...
make dist             # produce a self-contained binary with all assets packed via stuffbin
make release-dry      # goreleaser dry run, no publish
```

Docker dev environment (Postgres + app container):

```bash
make build-dev-docker
make run-dev-docker   # up + shell into the dev container
make stop-dev-docker  # stop, keep DB volume
make rm-dev-docker    # stop and delete the volume
```

End-to-end tests live in `tests/` (Playwright; see `tests/README.md`).

### Deployment notes

- `docker-compose.yml` runs the app + Postgres and passes config via
  `LISTMONK_*` env vars. `LISTMONK_*_FILE` variants read values from files
  (Docker/Podman secrets).
- `--install --idempotent --yes` sets up the DB on first start; `--upgrade`
  runs migrations on subsequent image pulls. Both are idempotent.
- `listmonk-simple.service` / `listmonk@.service` are systemd units for
  binary installs (`config.toml` at `/etc/listmonk/config.toml`).
- `Dockerfile` + `docker-entrypoint.sh` build the release image; the
  entrypoint supports `PUID`/`PGID` and `*_FILE` secret loading.
- In production, put a reverse proxy in front of `:9000` and configure an
  SMTP provider (or SES webhook bounce tracking) under Admin → Settings.

### Caveats

- Upstream PR branches must be cut from `upstream/master`, not local
  `master` — local master carries biofool-only files (`AGENTS.md`,
  `CLAUDE.md`, `.devin/`, `.ai-context/`, docs) that must not leak into
  upstream diffs. See `CLAUDE.md` → "Project-specific: listmonk".
- The admin frontend is server-side rendered (Go templates + Alpine.js),
  not the Vue SPA some older upstream docs describe.
- AGPLv3 license — modified network deployments must offer source.

## Developers
listmonk is free and open source software licensed under AGPLv3. If you are interested in contributing, refer to the [developer setup](https://listmonk.app/docs/developer-setup). The backend is written in Go and the admin frontend is server-rendered HTML (Go templates, Alpine.js, CodeMirror) built with Bun.


## License
listmonk is licensed under the AGPL v3 license.
