---
title: "SvelteKit #15511"
slug: sveltejs-kit-15511
repository: sveltejs/kit
issue_url: https://github.com/sveltejs/kit/issues/15511
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-19
---
# SvelteKit #15511

## Public case summary

- Repository: `sveltejs/kit`
- Issue: https://github.com/sveltejs/kit/issues/15511
- Pull request: https://github.com/sveltejs/kit/pull/16423
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reports rare corrupted `__data.json` responses during page
  invalidation, especially with streamed promises and slower network conditions.
- The visible failure showed corrupted JSON text reaching `JSON.parse` during
  client-side `load_data` processing.
- The issue was public, open, and labeled `awaiting submitter` at record time.
- The useful boundary was the streamed response path from raw bytes to UTF-8
  text, then newline-delimited JSON records, then `JSON.parse`.
- The repair area was the byte-to-text transition for streamed NDJSON data. A
  malformed byte sequence should fail at decoding time instead of becoming
  replacement text that later appears as corrupted JSON.

## Repair scope

- Decode streamed NDJSON with fatal UTF-8 handling.
- Keep the generic stream reader reusable by accepting optional
  `TextDecoderOptions`.
- Use fatal decoding for `read_ndjson`, while leaving the existing SSE stream
  reader behavior unchanged.
- Add regression coverage for split UTF-8 code points across chunks.
- Add regression coverage proving malformed UTF-8 rejects before JSON parsing.
- Not claimed: This does not provide a reliable end-to-end reproduction of the
  original network/timing conditions reported in the issue.
- Not claimed: This does not change SvelteKit's JSON parser or streamed data
  response format.

## Validation record

- Public contribution branch was based on SvelteKit's public `version-3`
  branch.
- Latest public pull request head recorded here:
  `0af8634102c75e9d40009da2919bebb0db9b3019`.
- Package unit tests passed: `pnpm -F @sveltejs/kit test:unit`.
- Repository lint passed after rerunning with a larger Node heap because the
  default local heap hit an out-of-memory condition during repo-wide ESLint.
- Repository check passed: `pnpm check`.
- Whitespace hygiene passed: `git diff --check`.
- A package-level browser/integration run reached Chromium Playwright tests but
  failed in `packages/kit/test/apps/async` on an existing direct `http.get` /
  Playwright webServer readiness case with `ECONNREFUSED 127.0.0.1:5173`.
  That area was not changed in this repair.
- Public CI visible shortly after opening included passing `lint-all`,
  cross-browser, server-side route-resolution, Svelte async, and `test-others`
  jobs, with some `test-kit` and preview jobs still pending at record time.

## Public review status

- The pull request was opened from the public `scarab-systems/kit` fork.
- The pull request targets `sveltejs/kit:version-3`.
- The pull request was open and not draft at record time.
- Maintainer edits were enabled at record time.
- The pull request is framed as related to sveltejs/kit#15511 because the full
  issue conditions were not reliably reproduced.

## Public links

- https://github.com/sveltejs/kit/issues/15511
- https://github.com/sveltejs/kit/pull/16423

## Changed public files

- packages/kit/src/runtime/client/ndjson.js
- packages/kit/src/runtime/client/ndjson.spec.js
- packages/kit/src/runtime/client/stream.js

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
