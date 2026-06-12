---
title: "Docker Compose #13613"
slug: docker-compose-13613
repository: docker/compose
issue_url: https://github.com/docker/compose/issues/13613
mode: repair
status: upstream-accepted
recorded_at: 2026-06-12
---
# Docker Compose #13613

## Public case summary

- Repository: `docker/compose`
- Issue: https://github.com/docker/compose/issues/13613
- Pull request: https://github.com/docker/compose/pull/13831
- Mode: repair
- Status: upstream-accepted

## Diagnostic finding

- None recorded in this case.

## Repair scope

- Compose variable extraction over unresolved config values that are intentionally still interpolation expressions
- Load non-interpolated models for variable discovery with compose-go validation skipped, preserving validation for normal config rendering while allowing templated typed fields such as port host_ip and published to be inspected for variables.
- Not claimed: The repair does not change normal interpolated config validation.
- Not claimed: The repair does not force local-only customization for OCI-distributed Compose applications.

## Validation record

- Regression before repair: go test ./cmd/compose -run 'Test(ExtractInterpolationVariablesFromModelAllowsTemplatedPortFields|RunVariablesAllowsTemplatedPortFields)' -count=1
- Result before repair: failed before repair with invalid ip address errors for ${LXKNS_ADDRESS:-127.0.0.1}
- Focused regression: go test ./cmd/compose -run 'Test(ExtractInterpolationVariablesFromModelAllowsTemplatedPortFields|RunVariablesAllowsTemplatedPortFields)' -count=1
- Focused result: passed
- Package test: go test ./cmd/compose
- Package result: passed
- Lint: docker buildx bake lint
- Lint result: passed with 0 issues
- Diff check: git diff --check
- Diff check result: passed

## Public review status

- Upstream pull request docker/compose#13831 was merged on 2026-06-12.
- Merge commit: docker/compose@6ded3140df8da4997fbd111bd7e21b41ec0f8891
- Reviewer-requested coverage for the remote-stack `up` prompt path was included before merge.

## Public links

- https://github.com/docker/compose/issues/13613
- https://github.com/docker/compose/pull/13831

## Changed public files

- cmd/compose/compose.go
- cmd/compose/config.go
- cmd/compose/options.go
- cmd/compose/options_test.go
- cmd/compose/up_test.go

## Record limits

This case publishes public links, findings, validation summaries, and status only.
