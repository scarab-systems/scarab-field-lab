---
title: "pnpm #12222"
slug: pnpm-pnpm-12222
repository: pnpm/pnpm
issue_url: https://github.com/pnpm/pnpm/issues/12222
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-11
---
# pnpm #12222

## Public case summary

- Repository: `pnpm/pnpm`
- Issue: https://github.com/pnpm/pnpm/issues/12222
- Pull request: https://github.com/pnpm/pnpm/pull/12327
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- SDS surfaced a runtime temporary-directory path-budget boundary around pnpm's content-addressable file store and package preparation flow.
- The public issue described IPC socket paths exceeding the Unix-domain socket path limit when pnpm runs as root with a long temporary directory path.
- The bounded repair stayed in pnpm's CAFS temporary package directory creation path rather than changing lifecycle privilege behavior, `unsafe-perm`, or issue-specific error handling.

## Repair scope

- Replace long `path-temp` CAFS temporary package directory names with `fs.mkdtemp()` using a compact `_tmp_` prefix under the existing store `tmp` directory.
- Remove the now-unneeded `path-temp` dependency from `@pnpm/store.create-cafs-store`.
- Add a regression test proving CAFS temp directories stay short enough to preserve path budget for downstream lifecycle IPC socket paths.
- Not claimed: This does not change npm lifecycle privilege-dropping behavior.
- Not claimed: This does not change `unsafe-perm` defaults.
- Not claimed: This does not change pacquet; its git fetcher path already uses a temporary-directory API with short generated names.

## Validation record

- Red check: the new git-fetcher regression failed when CAFS temp directory creation was temporarily changed back to a long suffix.
- Bundle compile: `pnpm --filter pnpm compile`
- Bundle compile result: passed with existing lint warnings and no errors.
- Store package test: `pnpm --filter @pnpm/store.create-cafs-store test`
- Store package test result: passed.
- Prepare-package test: `pnpm --filter @pnpm/exec.prepare-package test`
- Prepare-package test result: passed.
- Git fetcher test: `pnpm --filter @pnpm/fetching.git-fetcher test`
- Git fetcher test result: passed; 16 tests.
- Pre-push hook: passed after using the repository-required Rust toolchain first in `PATH`; optional tools skipped by the hook were not required for this patch.
- Public PR checks at recording: CodeQL, JavaScript analysis, dependency audit, compile/lint, zizmor, and CodeRabbit passed; Node.js 24 test and benchmark jobs were still pending.

## Public review status

- CodeRabbit review completed with no actionable comments.
- Qodo review reported no bugs, rule violations, or requirement gaps.
- No unresolved inline review threads were present at recording.
- Maintainer review was still pending at recording.

## Public links

- https://github.com/pnpm/pnpm/issues/12222
- https://github.com/pnpm/pnpm/pull/12327

## Changed public files

- .changeset/short-pipes-dance.md
- fetching/git-fetcher/test/index.ts
- pnpm-lock.yaml
- store/create-cafs-store/package.json
- store/create-cafs-store/src/index.ts

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
