---
title: "Kubernetes #138728"
slug: kubernetes-kubernetes-138728
repository: kubernetes/kubernetes
issue_url: https://github.com/kubernetes/kubernetes/issues/138728
mode: diagnostic-proof-and-repair
status: repair-recorded
recorded_at: 2026-06-06
---
# Kubernetes #138728

## Public case summary

- Repository: `kubernetes/kubernetes`
- Issue: https://github.com/kubernetes/kubernetes/issues/138728
- Mode: diagnostic-proof-and-repair
- Status: repair-recorded

## Diagnostic finding

- SDS surfaced cache/watch consistency and critical-section evidence in the Kubernetes API machinery target without treating issue text as diagnostic truth.
- Any repair work should start from the public watch-cache lock-hold and collection-scale snapshot/list boundary.

## Repair scope

- WatchList initial-events interval construction can materialize a large ordered store list while the watch-cache read lock is held for snapshot-to-watcher setup.
- For ordered stores, clone the ordered store snapshot while constructing the cache interval and defer the full ordered list materialization until the interval is first consumed by the watcher goroutine.
- Not claimed: No watch-cache event ordering semantics are changed.
- Not claimed: No watcher registration is moved outside the watch-cache lock.
- Not claimed: No broad cache architecture redesign is included.

## Validation record

- `go test ./staging/src/k8s.io/apiserver/pkg/storage/cacher -run 'TestCacheInterval(NextFromStore|FromOrderedStoreDefersListUntilNext|FromStoreSorted)$' -count=1` - passed
- `go test ./staging/src/k8s.io/apiserver/pkg/storage/cacher/store -count=1` - passed
- `go test ./staging/src/k8s.io/apiserver/pkg/storage/cacher -count=1` - passed
- `git diff --check` - passed

## Public links

- https://github.com/kubernetes/kubernetes/issues/138728

## Changed public files

- staging/src/k8s.io/apiserver/pkg/storage/cacher/watch_cache_interval.go
- staging/src/k8s.io/apiserver/pkg/storage/cacher/watch_cache_interval_test.go

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
