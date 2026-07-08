---
title: "pnpm #9191"
slug: pnpm-pnpm-9191
repository: pnpm/pnpm
issue_url: https://github.com/pnpm/pnpm/issues/9191
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-07
---
# pnpm #9191

## Public case summary

- Repository: `pnpm/pnpm`
- Issue: https://github.com/pnpm/pnpm/issues/9191
- Pull request: https://github.com/pnpm/pnpm/pull/12841
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported `ERR_PNPM_NO_GLOBAL_BIN_DIR` in a GitHub Actions
  workflow after `corepack enable`, `pnpm setup`, and a later global `pnpm`
  command.
- The repair area centered on setup environment propagation for GitHub Actions:
  a setup step can compute and apply `PNPM_HOME`, but later workflow steps only
  receive that value when the action writes to GitHub's environment and path
  files.
- The same boundary applies to the Rust `pacquet/` setup port, so both setup
  implementations needed aligned handling.

## Repair scope

- Write `PNPM_HOME` to the GitHub Actions environment file when `GITHUB_ENV` is
  provided.
- Write the setup bin directory to the GitHub Actions path file when
  `GITHUB_PATH` is provided.
- Keep the behavior conditional so non-GitHub setup paths are unchanged.
- Keep the two GitHub Actions file writes independent: a failure writing one
  target should not prevent the other target from being attempted.
- Preserve GitHub Actions line-oriented file parsing by starting appended
  records on a new line when an existing file is not newline-terminated.
- Use append-mode writes and regular-file checks for the TypeScript and Rust
  setup implementations, with Unix no-follow handling in the Rust path.
- Add focused TypeScript and Rust setup tests for the GitHub Actions
  environment-file behavior, error handling, and append formatting.
- Not claimed: This does not redesign pnpm's global-bin-dir detection.
- Not claimed: This does not change non-GitHub shell-profile setup behavior.
- Not claimed: pnpm/pnpm#12841 has not merged at recording.

## Validation record

- Public contribution branch was based on pnpm's public `main` branch.
- Latest public pull request head recorded here:
  `b88165064f047a4d00ede017c1a64df7dffb2b6f`.
- TypeScript setup test passed: `jest test/setup/setup.test.ts --runInBand`.
- TypeScript setup test result: 12 passed, 0 failed.
- Rust setup tests passed:
  `cargo nextest run -p pacquet-cli cli_args::setup::tests`.
- Rust setup test result: 11 passed, 0 failed.
- Rust format check passed: `cargo fmt --check -p pacquet-cli`.
- Rust lint check passed:
  `cargo clippy --locked -p pacquet-cli --all-targets -- --deny warnings`.
- TypeScript build and lint passed for `@pnpm/engine.pm.commands`.
- Diff whitespace check passed:
  `git diff --check`
- The repository pre-push hook passed before the latest push.
- Independent container verification at the latest public head passed:
  `cargo nextest run -p pacquet-cli --lib cli_args::setup::tests`.
- Independent container verification result: 11 passed, 0 failed.
- Public pull request status at recording: open, not draft, with upstream CI
  still queued or in progress.
- Public checks visible at recording: several upstream CI jobs were still
  pending. CodeRabbit's review status was approved.
- Not claimed: This record does not claim upstream review, CI completion, or
  merge.

## Public review status

- pnpm/pnpm#12841 is open against `pnpm:main`.
- The pull request was opened from the public `scarab-systems/pnpm` fork.
- The pull request is linked as fixing pnpm/pnpm#9191.
- A public reply was posted to the final CodeRabbit thread with current-head
  test evidence for the root-agnostic Rust setup test.
- After that reply, CodeRabbit approved the review and no unresolved review
  threads were visible. GitHub's review-decision field moved to review
  required, with upstream maintainer review and CI still pending.

## Public links

- https://github.com/pnpm/pnpm/issues/9191
- https://github.com/pnpm/pnpm/pull/12841
- https://github.com/pnpm/pnpm/pull/12841#discussion_r3538226796

## Changed public files

- .changeset/pnpm-setup-github-actions.md
- Cargo.lock
- pacquet/crates/cli/Cargo.toml
- pacquet/crates/cli/src/cli_args/setup.rs
- pacquet/crates/cli/src/cli_args/setup/tests.rs
- pacquet/crates/cmd-shim/src/shim.rs
- pnpm11/engine/pm/commands/src/setup/setup.ts
- pnpm11/engine/pm/commands/test/setup/setup.test.ts

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
