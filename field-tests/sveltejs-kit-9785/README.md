---
title: "SvelteKit #9785"
slug: sveltejs-kit-9785
repository: sveltejs/kit
issue_url: https://github.com/sveltejs/kit/issues/9785
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-07
---
# SvelteKit #9785

## Public case summary

- Repository: `sveltejs/kit`
- Issue: https://github.com/sveltejs/kit/issues/9785
- Pull request: https://github.com/sveltejs/kit/pull/16268
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reports that rejected streamed data promises can produce
  unhandled promise rejections or server crashes, and that page-level
  `{#await ... {:catch ...}}` handling does not reliably receive the error.
- The issue is publicly labeled `bug`, `error handling`, and `p1-important`.
- The repair area centered on SvelteKit server data serialization for streamed
  server data.
- The timing boundary was that the JSON data serializer attached stream
  rejection handling after all server load promises had settled.
- If one load returned a rejected streamed promise while another load was still
  pending, the rejection could occur before the serializer attached the stream
  rejection handler.

## Repair scope

- Create the JSON data serializer earlier in server data rendering.
- Add each server load result to the serializer as that load resolves.
- Preserve the final response ordering and data envelope by still waiting for
  all load promises before emitting the data response.
- Add a regression route where one server load delays serialization while
  another load returns a rejected streamed promise.
- Add a Playwright regression test that verifies eager data renders and the
  streamed rejection reaches the page catch block.
- Not claimed: This does not change the documented limitation that redirects
  cannot be thrown from streamed promises after the response has started.
- Not claimed: This does not redesign SvelteKit's streamed data API.
- Not claimed: sveltejs/kit#16268 has not merged at recording.

## Validation record

- Public contribution branch was based on SvelteKit's public `main` branch.
- Latest public pull request head recorded here:
  `83f2ce23cdbd0e72fb7aa4a11d5e62237988fd3c`.
- Format check passed on touched files:
  `pnpm exec prettier --check ...`
- Package check passed: `pnpm --dir packages/kit check`.
- Basics app check passed:
  `pnpm --dir packages/kit/test/apps/basics check`.
- Repository lint passed: `pnpm run lint`.
- Repository check passed: `pnpm run check`.
- Focused Playwright regression passed in dev mode.
- Focused Playwright regression passed in build/preview mode.
- A reduced full `pnpm test:kit` run was attempted. The new regression passed
  inside that run, but the full command failed on an existing unrelated no-SSR
  dev test. That unrelated failure was not changed in this repair.
- Public pull request status at recording: open and not draft, with upstream
  review required.
- Public checks visible at recording: several upstream CI jobs had passed and
  several remained pending. No failed GitHub checks were visible at recording.
- Not claimed: This record does not claim upstream review, CI completion, or
  merge.

## Public review status

- sveltejs/kit#16268 is open against `sveltejs/kit:main`.
- The pull request was opened from the public `scarab-systems/kit` fork.
- The pull request is linked as fixing sveltejs/kit#9785.
- GitHub's review-decision field showed review required at recording.
- GitHub's merge-state field showed blocked while review and checks were still
  pending.

## Public links

- https://github.com/sveltejs/kit/issues/9785
- https://github.com/sveltejs/kit/pull/16268

## Changed public files

- .changeset/quiet-masks-shop.md
- packages/kit/src/runtime/server/data/index.js
- packages/kit/test/apps/basics/src/routes/streaming/+page.svelte
- packages/kit/test/apps/basics/src/routes/streaming/server/delayed-rejection/+layout.server.js
- packages/kit/test/apps/basics/src/routes/streaming/server/delayed-rejection/+page.server.js
- packages/kit/test/apps/basics/src/routes/streaming/server/delayed-rejection/+page.svelte
- packages/kit/test/apps/basics/test/client.test.js

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
