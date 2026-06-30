---
title: "Docker Compose #13602"
slug: docker-compose-13602
repository: docker/compose
issue_url: https://github.com/docker/compose/issues/13602
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-30
---
# Docker Compose #13602

## Public case summary

- Repository: `docker/compose`
- Issue: https://github.com/docker/compose/issues/13602
- Pull request: https://github.com/docker/compose/pull/13889
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported that a long-syntax bind mount with
  `bind.create_host_path: false` still created a missing host path and allowed
  `docker compose run --rm` to start the service.
- The reporter also noted that `docker compose config` preserved
  `create_host_path: false`, so the value was parsed but not enforced during
  runtime mount handling.
- The repair boundary was Compose-side bind-source validation before container
  creation, not Docker Engine or Docker Desktop path-creation behavior.

## Repair scope

- Validate the resolved bind source path when a bind mount explicitly sets
  `bind.create_host_path: false`.
- Return a missing bind source error before container creation when the source
  path does not exist.
- Keep default bind behavior and ordinary host-path creation behavior outside
  this explicit `create_host_path: false` path.
- Not claimed: docker/compose#13889 has not merged at recording.
- Not claimed: This record does not claim a Docker Engine or Docker Desktop
  runtime change.

## Validation record

- Regression before repair:
  `go test ./pkg/compose -run TestBuildContainerVolumesReturnsErrorWhenBindSourceMissingAndCreateHostPathFalse -count=1`.
- Result before repair: failed because the missing bind source did not return
  an error.
- Focused unit coverage passed:
  `go test ./pkg/compose -run 'TestBuildContainerVolumesReturnsErrorWhenBindSourceMissingAndCreateHostPathFalse|Test_buildContainerVolumes' -count=1`.
- Contributor build passed:
  `go build -trimpath -tags e2e -o bin/build/docker-compose ./cmd`.
- Focused end-to-end coverage passed:
  `go test -v -count=1 ./pkg/e2e -run TestRunDoesNotCreateMissingBindSourceWhenCreateHostPathIsFalse`.
- Maintainer lint target passed:
  `docker buildx bake lint`.
- Public checks visible at recording: DCO passed.

## Public review status

- docker/compose#13889 is open against `docker/compose:main`.
- The pull request was opened from the public `scarab-systems/compose` fork.
- The pull request is related to docker/compose#13602.
- Public status at recording: open, ready for review, and not merged.

## Public links

- https://github.com/docker/compose/issues/13602
- https://github.com/docker/compose/pull/13889

## Changed public files

- pkg/compose/create.go
- pkg/compose/create_test.go
- pkg/e2e/volumes_test.go

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
