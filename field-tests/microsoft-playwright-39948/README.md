---
title: "Playwright #39948"
slug: microsoft-playwright-39948
repository: microsoft/playwright
issue_url: https://github.com/microsoft/playwright/issues/39948
mode: repair
status: public-comment-recorded
recorded_at: 2026-06-10
last_updated: 2026-06-25
---
# Playwright #39948

## Public case summary

- Repository: `microsoft/playwright`
- Issue: https://github.com/microsoft/playwright/issues/39948
- Mode: repair
- Status: public-comment-recorded
- Latest public status: Playwright maintainers identified an upstream Chromium
  fix, so no Playwright PR was opened.

## Diagnostic finding

- The reported hang stayed tied to Chromium worker-main-script response header
  handling for a worker created inside an iframe.
- Playwright maintainers later pointed to an upstream Chromium fix as the owner
  layer for the issue.

## Repair scope

- Chromium worker-main-script response headers when a worker is created inside an iframe.
- Preserve normal Chromium extra-info waiting behavior, but allow a worker-frame terminal event to resolve missing raw request and response headers from provisional headers when Chromium reports hasExtraInfo without delivering matching extra-info events.
- Not claimed: No Playwright PR was opened because CONTRIBUTING.md requires assignment or explicit community approval before community PR work.
- Not claimed: After maintainers identified the upstream Chromium fix, the
  Playwright-side patch was not pursued.
- Not claimed: The repair does not broaden all response waiting semantics; the fallback is tied to worker-frame completion.

## Validation record

- `npm run build && npm run ctest -- tests/page/workers.spec.ts -g "should report all response headers for worker script inside iframe" --workers=1 --reporter=line` - failed without the source fix because headers remained null after the 1000ms guard
- `npm run build && npm run ctest -- tests/page/workers.spec.ts -g "should report all response headers for worker script inside iframe" --workers=1 --reporter=line` - passed with the source fix
- `npm run ctest -- tests/page/workers.spec.ts --workers=1 --reporter=line` - passed; 21 passed, 1 existing skip
- `npm run ctest -- tests/page/page-network-response.spec.ts --workers=1 --reporter=line` - passed; 24 passed
- `npx eslint packages/playwright-core/src/server/chromium/crNetworkManager.ts tests/page/workers.spec.ts` - passed
- `npm run tsc` - passed
- `npm run lint` - passed

## Public links

- https://github.com/microsoft/playwright/issues/39948
- https://github.com/microsoft/playwright/issues/39948#issuecomment-4665509609
- https://github.com/microsoft/playwright/issues/39948#issuecomment-4802606264
- https://github.com/microsoft/playwright/issues/39948#issuecomment-4802790869
- https://chromium-review.googlesource.com/c/chromium/src/+/8003476

## Affected public files

- packages/playwright-core/src/server/chromium/crNetworkManager.ts
- tests/page/workers.spec.ts

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
