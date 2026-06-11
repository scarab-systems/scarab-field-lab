---
title: "Next.js #94450"
slug: vercel-next-js-94450
repository: vercel/next.js
issue_url: https://github.com/vercel/next.js/issues/94450
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-06
---
# Next.js #94450

## Public case summary

- Repository: `vercel/next.js`
- Issue: https://github.com/vercel/next.js/issues/94450
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- SDS surfaced source-map artifact provenance evidence in the Next.js target without treating issue text as diagnostic truth.
- The upstream repair branch focused on preserving original sourcesContent authority across the Babel loader transform map and Turbopack final browser source-map composition boundary.

## Repair scope

- React Compiler transform map to Turbopack final browser source-map provenance
- Preserve the original loader input in the Babel loader source map, then fill a missing Turbopack source-map file identity from the resolved origin path so final composition can match the transform map back to the generated intermediate source.
- Not claimed: The PR does not claim a broad source-map redesign.
- Not claimed: The repair is limited to preserving original-source authority across the existing transform/composition path.

## Validation record

- `scripts/run-jest.sh --mode=start --bundler=turbo --headless -- test/production/production-browser-sourcemaps -t "React Compiler"` - passed; React Compiler production browser source-map regression preserved original app/client.js sourcesContent
- `cargo test -p turbopack-core --lib source_map::utils::tests::test_resolve_source_map_sources` - passed
- `rustfmt --check turbopack/crates/turbopack-core/src/source_map/utils.rs` - passed

## Public links

- https://github.com/vercel/next.js/issues/94450
- https://github.com/vercel/next.js/pull/94509

## Changed public files

- packages/next/src/build/babel/loader/transform.ts
- turbopack/crates/turbopack-core/src/source_map/utils.rs
- test/production/production-browser-sourcemaps/app/client.js
- test/production/production-browser-sourcemaps/app/page.js
- test/production/production-browser-sourcemaps/index.test.ts

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
