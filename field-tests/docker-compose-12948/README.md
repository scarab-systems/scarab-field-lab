---
title: "Docker Compose #12948"
slug: docker-compose-12948
repository: docker/compose
issue_url: https://github.com/docker/compose/issues/12948
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-19
---
# Docker Compose #12948

## Public case summary

- Repository: `docker/compose`
- Issue: https://github.com/docker/compose/issues/12948
- Pull request: https://github.com/docker/compose/pull/13952
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue requests Docker CLI-style Go template formatting for
  `docker compose ls` and `docker compose images`.
- The reported user need is machine-readable project names without parsing the
  table output or depending on a JSON parser such as `jq`.
- Both commands already exposed `--format`, but their output path still used
  Compose's older table/json formatter rather than the Docker CLI template
  formatter used by sibling commands such as `docker compose ps`.
- The useful repair boundary was the command display formatting layer for
  Compose project and image rows, not Compose project discovery, image lookup,
  Docker Engine behavior, or container lifecycle code.

## Repair scope

- Add a typed formatter context for `docker compose ls` project rows.
- Add a typed formatter context for `docker compose images` image rows.
- Route custom templates through Docker CLI's formatter machinery.
- Update the `--format` help text for both commands to the standard Docker CLI
  template-formatting help text.
- Add regression coverage for custom templates and `{{json .}}` template
  rendering.
- Preserve existing `--format json` array output for compatibility while
  making template JSON available through `{{json .}}`.
- Not claimed: docker/compose#13952 has not merged at recording.
- Not claimed: This record does not claim a change to top-level Docker CLI
  commands.
- Not claimed: This record does not claim a maintainer decision on changing the
  existing `--format json` output shape for these Compose commands.

## Validation record

- Public contribution branch was based on Docker Compose's public `main`
  branch.
- Latest public pull request head recorded here:
  `66d3e9e1315d79caaeec2b66a028af29c655ef67`.
- Formatting passed:
  `gofmt -s -w cmd/compose/list.go cmd/compose/images.go cmd/formatter/formats.go cmd/formatter/project.go cmd/formatter/image.go cmd/formatter/compose_format_test.go`.
- Command package tests passed: `go test ./cmd/...`.
- Whitespace hygiene passed: `git diff --check`.
- Public checks visible at recording: DCO passed.

## Public review status

- docker/compose#13952 is open against `docker/compose:main`.
- The pull request was opened from the public `scarab-systems/compose` fork.
- The pull request is related to docker/compose#12948.
- Public status at recording: open, ready for review, and not merged.
- GitHub review status at recording: review required.
- GitHub merge-state field at recording: blocked.
- Maintainer edits were enabled at recording.

## Public links

- https://github.com/docker/compose/issues/12948
- https://github.com/docker/compose/pull/13952

## Changed public files

- cmd/compose/images.go
- cmd/compose/list.go
- cmd/formatter/compose_format_test.go
- cmd/formatter/formats.go
- cmd/formatter/image.go
- cmd/formatter/project.go

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
