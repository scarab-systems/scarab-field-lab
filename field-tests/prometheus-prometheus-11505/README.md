---
title: "Prometheus #11505"
slug: prometheus-prometheus-11505
repository: prometheus/prometheus
issue_url: https://github.com/prometheus/prometheus/issues/11505
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-19
---
# Prometheus #11505

## Public case summary

- Repository: `prometheus/prometheus`
- Issue: https://github.com/prometheus/prometheus/issues/11505
- Pull request: https://github.com/prometheus/prometheus/pull/18978
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported that Prometheus remote-write ingestion should verify
  that incoming series labels are sorted lexicographically.
- The repair area centered on the remote-write handler before incoming label
  data is converted into Prometheus' internal label representation.
- Without an explicit pre-conversion ordering check, incoming unsorted label
  data could be normalized by the conversion path instead of being rejected as
  invalid remote-write input.

## Repair scope

- Add remote-write v1 label-order validation before v1 samples are appended.
- Add remote-write v2 label-reference order validation before v2 samples are
  appended.
- Route v1 unsorted-label series through the existing invalid-label skip path.
- Route v2 unsorted-label series through the existing partial-write bad-request
  path.
- Add regression coverage for both v1 and v2 remote-write requests.
- Not claimed: This does not redesign remote-write request validation.
- Not claimed: This does not change remote-read behavior.
- Not claimed: prometheus/prometheus#18978 has not merged at recording.

## Validation record

- Contribution branch was based on Prometheus' public `main` branch.
- Full maintainer test target passed in a Linux arm64 container:
  `make test`.
- Maintainer lint target passed in the same container:
  `make lint`.
- Focused remote-write regression package passed:
  `go test ./storage/remote -count=1`.
- Public pull request status at recording: open, mergeable, and ready for
  review.
- Public checks visible at recording: DCO passed; Netlify deploy-preview status
  was successful, with related Netlify informational checks completed.
- Not claimed: This record does not claim upstream review or merge.

## Public review status

- prometheus/prometheus#18978 is open against `prometheus:main`.
- The pull request was opened from the public `scarab-systems/prometheus` fork.
- The pull request is linked by GitHub as closing prometheus/prometheus#11505.

## Public links

- https://github.com/prometheus/prometheus/issues/11505
- https://github.com/prometheus/prometheus/pull/18978

## Changed public files

- storage/remote/write_handler.go
- storage/remote/write_handler_test.go

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
