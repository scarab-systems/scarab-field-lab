---
title: "Prometheus #12244"
slug: prometheus-prometheus-12244
repository: prometheus/prometheus
issue_url: https://github.com/prometheus/prometheus/issues/12244
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-19
---
# Prometheus #12244

## Public case summary

- Repository: `prometheus/prometheus`
- Issue: https://github.com/prometheus/prometheus/issues/12244
- Pull request: https://github.com/prometheus/prometheus/pull/18979
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported confusion around Docker Swarm task discovery
  container labels.
- Public repository evidence showed that the documented
  `__meta_dockerswarm_container_label_<labelname>` metadata is populated from
  Swarm task container-spec labels.
- The repair area was documentation, because the current discovery behavior does
  not consistently imply runtime Docker container or image label inspection.

## Repair scope

- Clarify the documented source of Docker Swarm task container label metadata.
- Keep the change scoped to service-discovery configuration documentation.
- Not claimed: This does not add new Docker API calls.
- Not claimed: This does not change Docker Swarm service-discovery runtime
  behavior.
- Not claimed: prometheus/prometheus#18979 has not merged at recording.

## Validation record

- Contribution branch was based on Prometheus' public `main` branch.
- Focused Docker Swarm discovery package passed:
  `go test ./discovery/moby -count=1`.
- Prometheus contributor build command passed:
  `go build ./cmd/prometheus/`.
- Go-only maintainer test target passed:
  `GO_ONLY=1 make test`.
- Maintainer lint target passed:
  `make lint`.
- Full maintainer test target passed:
  `make test`.
- Public pull request status at recording: open, ready for review, and review
  required.
- Public checks visible at recording: DCO passed; Netlify checks had no failing
  status.
- Not claimed: This record does not claim upstream review or merge.

## Public review status

- prometheus/prometheus#18979 is open against `prometheus:main`.
- The pull request was opened from the public `scarab-systems/prometheus` fork.
- The pull request is related to prometheus/prometheus#12244.

## Public links

- https://github.com/prometheus/prometheus/issues/12244
- https://github.com/prometheus/prometheus/pull/18979
- https://github.com/prometheus/prometheus/issues/12244#issuecomment-4755335789

## Changed public files

- docs/configuration/configuration.md

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
