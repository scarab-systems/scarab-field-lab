---
title: "Next.js #54482"
slug: vercel-next-js-54482
repository: vercel/next.js
issue_url: https://github.com/vercel/next.js/issues/54482
mode: diagnostic-proof-and-repair
status: repair-recorded
recorded_at: 2026-06-04
---
# Next.js #54482

## Public case summary

- Repository: `vercel/next.js`
- Issue: https://github.com/vercel/next.js/issues/54482
- Mode: diagnostic-proof-and-repair
- Status: repair-recorded

## Diagnostic finding


## Repair scope

- Replace unbounded optimized-output buffering with a bounded async-stream collector tied to the existing maximumResponseBody budget, while preserving the final Buffer shape used by cache and ETag behavior.
- High memory usage or suspected memory leak when Next.js image optimization is enabled.
- The production image optimizer path buffered Sharp's optimized output with toBuffer before enforcing a response-body budget.
- Not claimed: Do not redesign Next.js image optimization caching.
- Not claimed: Do not change upstream input-fetch limits outside this repair boundary.
- Not claimed: Do not claim to resolve every memory report linked from the issue.
- Not claimed: Do not chase example, script, or tracing-fixture media-transform findings in this repair slice.

## Validation record

- Focused unit: pnpm testonly test/unit/image-optimizer/optimize-image.test.ts --runInBand
- Focused unit result: 1 suite, 2 tests passed
- Image optimizer suite: pnpm testonly test/unit/image-optimizer --runInBand
- Image optimizer result: 12 suites, 141 tests passed
- Types: pnpm --filter=next types
- Types result: passed
- Prettier: pnpm prettier --check packages/next/src/server/image-optimizer.ts test/unit/image-optimizer/optimize-image.test.ts
- Prettier result: All matched files use Prettier code style!
- Diff check: git diff --check
- Diff check result: passed

## Public links

- https://github.com/vercel/next.js/issues/54482

## Changed public files

- packages/next/src/server/image-optimizer.ts
- test/unit/image-optimizer/optimize-image.test.ts

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
