---
title: "Docker Compose #13613"
slug: docker-compose-13613
repository: docker/compose
issue_url: https://github.com/docker/compose/issues/13613
mode: repair
status: upstream-pr-recorded
recorded_at: 2026-06-09
---
# Docker Compose #13613

## Public case summary

- Repository: `docker/compose`
- Issue: https://github.com/docker/compose/issues/13613
- Mode: repair
- Status: upstream-pr-recorded

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

## Public links

- https://github.com/docker/compose/issues/13613
- https://github.com/docker/compose/pull/13831

## Changed public files

- cmd/compose/config.go
- cmd/compose/options.go
- cmd/compose/options_test.go

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
