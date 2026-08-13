---
title: "Deno #30652"
slug: denoland-deno-30652
repository: denoland/deno
issue_url: https://github.com/denoland/deno/issues/30652
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-06
---
# Deno #30652

## Public case summary

- Repository: `denoland/deno`
- Issue: https://github.com/denoland/deno/issues/30652
- Pull request: https://github.com/denoland/deno/pull/36350
- Superseded pull request: https://github.com/denoland/deno/pull/34979
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded
- Upstream status: refreshed open pull request recorded on 2026-07-29.

## Diagnostic finding

- The public issue reports that `Deno.stdin.readable.cancel()` can leave an
  `op_read` alive after JavaScript resolves, causing the process to wait for
  later stdin input instead of exiting cleanly.
- The useful repair boundary was Deno stdin's resource-backed readable stream
  lifecycle and native read ref/unref behavior.
- The upstream repair preserves Deno stdin stream semantics while ensuring
  readable cancellation releases pending native read lifecycle authority so the
  process can exit without later stdin input.

## Repair scope

- Route `Deno.stdin.readable` through the existing unrefable resource-backed
  stream path.
- Keep stdin ref/unref hooks synchronized with that stream.
- Unref pending unrefable reads during stream cancellation.
- Avoid parking Unix stdin async reads indefinitely inside a blocking read when
  no input is available.
- Preserve normal EOF/error behavior so shared stdin remains readable after EOF
  and cancellation cases covered by the refreshed PR.
- Not claimed: The PR does not redesign Deno streams or stdio.
- Not claimed: The repair is limited to stdin readable cancellation and existing resource-backed stream lifecycle behavior.

## Validation record

- Public contribution branch was refreshed against Deno's public `main` branch.
- Latest public pull request head recorded here:
  `88cd2c5400f1d9b611269ca279d0671a1358f0a3`.
- Public pull request validation recorded: `./x fmt`.
- Public pull request validation recorded: `./x lint`.
- Public pull request validation recorded: `./x check`.
- Public pull request validation recorded: `./x build`.
- Public pull request validation recorded:
  `rustup run 1.95.0 cargo check -p deno_io -p deno_web -p integration_tests`.
- Public pull request validation recorded:
  `rustup run 1.95.0 cargo test -p integration_tests --test integration run::stdin_readable_cancel_exits -- --exact --nocapture`.
- Public pull request validation recorded:
  `rustup run 1.95.0 cargo test -p integration_tests --test integration run::stdin_readable_eof_keeps_stdin_readable -- --exact --nocapture`.
- Public pull request validation recorded:
  `rustup run 1.95.0 cargo test -p integration_tests --test integration run::stdin_readable_cancel_keeps_stdin_readable -- --exact --nocapture`.
- Public pull request validation recorded: `git diff --check`.
- Toolchain matched the project Rust configuration.

## Public review status

- denoland/deno#34979 was closed without merge after its branch could not be
  reopened cleanly.
- denoland/deno#36350 supersedes denoland/deno#34979 on a refreshed branch.
- denoland/deno#36350 is open against `denoland/deno:main`.
- The refreshed pull request was opened from the public `scarab-systems/deno`
  fork.
- Maintainer edits are enabled.
- denoland/deno#30652 remains open at recording.

## Public links

- https://github.com/denoland/deno/issues/30652
- https://github.com/denoland/deno/pull/34979
- https://github.com/denoland/deno/pull/36350

## Changed public files

- ext/io/12_io.js
- ext/io/lib.rs
- ext/web/06_streams.js
- tests/integration/run_tests.rs

## Assistance disclosure

The refreshed public pull request includes an AI-assisted coding disclosure.
Public submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
