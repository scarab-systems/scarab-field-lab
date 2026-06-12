---
title: "Next.js #81161"
slug: vercel-next-js-81161
repository: vercel/next.js
issue_url: https://github.com/vercel/next.js/issues/81161
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-12
---
# Next.js #81161

## Public case summary

- Repository: `vercel/next.js`
- Issue: https://github.com/vercel/next.js/issues/81161
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- SDS surfaced cache source-of-truth, cache freshness, and shared runtime
  artifact-cache authority evidence in the Next.js/Turbopack target without
  treating issue text as diagnostic truth.
- The bounded repair focused on Turbopack filesystem watcher work around denied
  generated output/cache paths, not on the broader memory-eviction or cache
  pruning system.

## Repair scope

- Recursive platform watchers can still receive native events for paths that the
  project filesystem already treats as denied.
- The patch filters denied filesystem paths before watcher events are queued for
  invalidation work.
- The patch preserves `RenameMode::Both` source/destination pairing before
  filtering so one denied side cannot turn a valid rename event into a malformed
  event.
- Not claimed: The repair does not claim to solve every Turbopack RAM, CPU, or
  cache-growth report connected to the issue.
- Not claimed: The repair does not replace Turbopack memory eviction, task
  eviction, or future cache pruning work.

## Validation record

- `cargo +nightly-2026-05-15 fmt --package turbo-tasks-fs --check` - passed
- `cargo +nightly-2026-05-15 test -p turbo-tasks-fs denied_path` - passed
- `cargo +nightly-2026-05-15 test -p turbo-tasks-fs` - passed; 113 unit tests
  and doc-tests completed
- `cargo +nightly-2026-05-15 clippy -p turbo-tasks-fs --tests -- -D warnings`
  - passed
- `git diff --check` - passed
- Commit signature check - passed; GitHub reported the PR commit as verified
- Public PR checks passed: Socket Security, Vercel Agent Review, and Vercel Code
  Owners

## Public links

- https://github.com/vercel/next.js/issues/81161
- https://github.com/vercel/next.js/pull/94735

## Changed public files

- turbopack/crates/turbo-tasks-fs/src/denied_paths.rs
- turbopack/crates/turbo-tasks-fs/src/lib.rs
- turbopack/crates/turbo-tasks-fs/src/watcher.rs

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status
only. Internal diagnostic artifacts and target worktrees are not published here.
Clean public patch verification is represented by repo-native tests and public
PR checks, not by internal post-patch diagnostic reruns.
