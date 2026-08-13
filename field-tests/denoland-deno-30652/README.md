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
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- SDS surfaced a resource-backed stream cancellation boundary from public repository evidence, without treating issue text as diagnostic truth.
- The upstream repair preserves Deno stdin stream semantics while ensuring readable cancellation releases pending native read lifecycle authority so the process can exit without later stdin input.

## Repair scope

- Deno stdin readable stream cancellation to native read/resource lifecycle authority
- Route Deno.stdin.readable through the existing unrefable resource-backed stream path, keep stdin ref/unref hooks synchronized with that stream, unref pending unrefable reads during stream cancellation, and avoid parking Unix stdin async reads indefinitely inside a blocking read when no input is available.
- Not claimed: The PR does not redesign Deno streams or stdio.
- Not claimed: The repair is limited to stdin readable cancellation and existing resource-backed stream lifecycle behavior.

## Validation record

- `./x fmt` - passed
- `./x lint` - passed
- `cargo test -p integration_tests --test integration run::stdin_readable_cancel_exits -- --exact --nocapture` - passed repeatedly, including three consecutive runs after lint
- Toolchain matched the project Rust configuration.

## Public links

- https://github.com/denoland/deno/issues/30652
- https://github.com/denoland/deno/pull/34979

## Changed public files

- ext/io/12_io.js
- ext/io/lib.rs
- ext/web/06_streams.js
- tests/integration/run_tests.rs

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
