---
title: "Prometheus #12244"
slug: prometheus-prometheus-12244
repository: prometheus/prometheus
issue_url: https://github.com/prometheus/prometheus/issues/12244
mode: diagnostic-proof-and-repair
status: upstream-closed
recorded_at: 2026-06-19
---
# Prometheus #12244

## Public case summary

- Repository: `prometheus/prometheus`
- Issue: https://github.com/prometheus/prometheus/issues/12244
- Pull request: https://github.com/prometheus/prometheus/pull/18979
- Mode: diagnostic-proof-and-repair
- Status: upstream-closed
- Review outcome: closed without merge after the issue was superseded by a
  separate runtime-fix patch path.

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
- Not claimed: prometheus/prometheus#18979 did not merge.
- Not claimed: This record does not claim the separate runtime-fix patch is
  merged or accepted.

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
- Public status refresh on 2026-08-04 found prometheus/prometheus#12244 closed.
- Public status refresh on 2026-08-05 found prometheus/prometheus#18979 closed
  without merge.
- Separate public patch path: prometheus/prometheus#19284 was open for
  prometheus/prometheus#12244 at the latest status refresh.
- Public checks visible at recording: DCO passed; Netlify checks had no failing
  status.
- Not claimed: This record does not claim upstream review or merge.

## Public review status

- prometheus/prometheus#18979 was opened against `prometheus:main`.
- The pull request was opened from the public `scarab-systems/prometheus` fork.
- The pull request is related to prometheus/prometheus#12244.
- Public status at latest refresh: closed without merge by the contributor after
  the issue was superseded by a separate patch path.
- prometheus/prometheus#12244 was closed on 2026-08-04.
- prometheus/prometheus#19284 was open at latest refresh as a separate runtime
  patch for prometheus/prometheus#12244.
- No upstream merge or acceptance is claimed for this case.

## Public links

- https://github.com/prometheus/prometheus/issues/12244
- https://github.com/prometheus/prometheus/pull/18979
- https://github.com/prometheus/prometheus/pull/19284
- https://github.com/prometheus/prometheus/issues/12244#issuecomment-4755335789

## Changed public files

- docs/configuration/configuration.md

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
