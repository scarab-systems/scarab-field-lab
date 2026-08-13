---
title: "Docker Compose #13649"
slug: docker-compose-13649
repository: docker/compose
issue_url: https://github.com/docker/compose/issues/13649
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-26
---
# Docker Compose #13649

## Public case summary

- Repository: `docker/compose`
- Issue: https://github.com/docker/compose/issues/13649
- Pull request: https://github.com/docker/compose/pull/13972
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded
- Upstream status: open pull request recorded on 2026-07-26.

## Diagnostic finding

- The public issue reports that `docker compose up` in an empty directory can
  report a missing compose config instead of surfacing the Docker socket
  permission error that prevents the command from talking to the daemon.
- The useful repair boundary was the `up` command's default config discovery
  and Docker API connection ordering.
- The narrow repair should only check the Docker API before returning the
  missing-config error when no compose file was explicitly selected and default
  discovery found no candidate to load.
- Explicit config paths, `COMPOSE_FILE`, env-file based configuration, `.env`
  defined `COMPOSE_FILE`, and existing default compose files should keep the
  normal Compose project-loading path.

## Repair scope

- Surface Docker connection errors before the missing-config error in the
  empty-directory default-discovery case.
- Preserve explicit config and environment-driven config loading order.
- Add command tests for Docker-connection-first behavior and config-load-before
  client-use behavior.
- Not claimed: This does not redesign Compose project loading.
- Not claimed: This does not change error ordering for explicit config inputs.

## Validation record

- Public contribution branch was based on Docker Compose's public `main` branch.
- Latest public pull request head recorded here:
  `a137970fe2c9955ec6606462874c537f6dc116da`.
- Public pull request validation recorded:
  `go test ./cmd/compose -run 'TestUp(ChecksDockerConnectionBeforeDefaultConfigDiscovery|LoadsConfigBeforeDockerConnection)$' -count=1`.
- Public pull request validation recorded:
  `go test ./cmd/compose ./pkg/compose -count=1`.
- Public pull request validation recorded: `make lint`.
- Public pull request validation recorded: `make test`.
- Public pull request validation recorded: `make validate`.
- Public pull request validation recorded: `make build`.
- Public pull request validation recorded manual smoke coverage for an empty
  directory plus unreadable Docker socket returning a Docker API permission
  error.
- Public pull request validation recorded manual smoke coverage preserving the
  `COMPOSE_FILE`-points-to-directory error.
- Public pull request validation recorded that `make build-and-e2e` was
  attempted locally and failed on unrelated Docker Desktop environment cases
  also reproduced on clean current `origin/main`.

## Public review status

- docker/compose#13972 is open against `docker/compose:main`.
- The pull request was opened from the public `scarab-systems/compose` fork.
- The pull request fixes docker/compose#13649.
- Maintainer edits are enabled.
- docker/compose#13649 remains open at recording.

## Public links

- https://github.com/docker/compose/issues/13649
- https://github.com/docker/compose/pull/13972

## Changed public files

- cmd/compose/compose_test.go
- cmd/compose/up.go

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
