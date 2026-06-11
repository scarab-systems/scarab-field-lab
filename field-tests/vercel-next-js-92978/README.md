---
title: "Next.js #92978"
slug: vercel-next-js-92978
repository: vercel/next.js
issue_url: https://github.com/vercel/next.js/issues/92978
mode: repair
status: upstream-pr-recorded
recorded_at: 2026-06-09
---
# Next.js #92978

## Public case summary

- Repository: `vercel/next.js`
- Issue: https://github.com/vercel/next.js/issues/92978
- Mode: repair
- Status: upstream-pr-recorded

## Diagnostic finding

- None recorded in this case.

## Repair scope

- Next.js workspace-root inference when both an application lockfile and a stray parent lockfile are present
- Select the nearest discovered lockfile as the inferred root marker and report farther lockfiles as additional detections in the duplicate-lockfile warning.
- Not claimed: The repair does not rely on users deleting parent lockfiles.
- Not claimed: The repair is limited to root selection and duplicate-lockfile warning semantics.

## Validation record

- `pnpm testonly packages/next/src/lib/find-root.test.ts` - passed
- `pnpm exec prettier --check packages/next/src/lib/find-root.ts packages/next/src/lib/find-root.test.ts` - passed
- `pnpm exec eslint --config eslint.cli.config.mjs packages/next/src/lib/find-root.ts packages/next/src/lib/find-root.test.ts` - passed
- `COREPACK_INTEGRITY_KEYS=0 pnpm --filter=next build` - passed; initial build attempts without the Corepack workaround reached compile/type subcommands but failed on local Corepack pnpm tarball key verification under Node 24

## Public links

- https://github.com/vercel/next.js/issues/92978
- https://github.com/vercel/next.js/pull/94597

## Changed public files

- packages/next/src/lib/find-root.ts
- packages/next/src/lib/find-root.test.ts

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
