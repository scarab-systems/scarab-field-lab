---
title: "SvelteKit #9785"
slug: sveltejs-kit-9785
repository: sveltejs/kit
issue_url: https://github.com/sveltejs/kit/issues/9785
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-07-07
---
# SvelteKit #9785

## Public case summary

- Repository: `sveltejs/kit`
- Issue: https://github.com/sveltejs/kit/issues/9785
- Pull request: https://github.com/sveltejs/kit/pull/16268
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted

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
- Not claimed: This record does not claim that sveltejs/kit#9785 was closed by
  this pull request.

## Validation record

- Public contribution branch was rebased onto SvelteKit's public `version-3`
  branch after maintainer review.
- Latest accepted public pull request head recorded here:
  `456e37877955e14003d12a30473b398375c67a98`.
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
- Public checks visible before merge included successful `lint-all`, `test-kit`,
  cross-browser, server-side route-resolution, Svelte async, and `test-others`
  CI jobs. No failed GitHub checks were visible at acceptance.

## Public review status

- The pull request was opened from the public `scarab-systems/kit` fork.
- During review, a SvelteKit maintainer clarified that the pull request
  addressed a distinct streamed-data rejection race demonstrated by the failing
  regression test.
- A SvelteKit maintainer requested retargeting the pull request onto the
  `version-3` branch; the branch was rebased and the pull request was retargeted.
- Rich Harris approved the pull request on 2026-07-14.
- The pull request was merged into `sveltejs/kit:version-3` on 2026-07-14.
- Merge commit: sveltejs/kit@9f3d9bbb0c63e7197c23f7529565542c2d77e592

## Public links

- https://github.com/sveltejs/kit/issues/9785
- https://github.com/sveltejs/kit/pull/16268
- https://github.com/sveltejs/kit/pull/16268#issuecomment-4970546974
- https://github.com/sveltejs/kit/pull/16268#pullrequestreview-4697542518

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
