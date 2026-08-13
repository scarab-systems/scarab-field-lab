---
title: "SvelteKit #14409"
slug: sveltejs-kit-14409
repository: sveltejs/kit
issue_url: https://github.com/sveltejs/kit/issues/14409
mode: diagnostic-proof-and-repair
status: upstream-closed
recorded_at: 2026-07-29
---
# SvelteKit #14409

## Public case summary

- Repository: `sveltejs/kit`
- Issue: https://github.com/sveltejs/kit/issues/14409
- Pull request: https://github.com/sveltejs/kit/pull/16559
- Mode: diagnostic-proof-and-repair
- Status: upstream-closed
- Upstream status: pull request closed without merge on 2026-07-29.

## Diagnostic finding

- The public issue reports remote function type export problems when building a
  SvelteKit library package with `@sveltejs/package`.
- The useful repair boundary was `svelte-package` declaration emit for packages
  whose library sources export remote functions from custom lib folders.
- Without generated `$app/types` in the emitter program, `query(...)` exports
  from `$app/server` can be emitted as `any` unless user code adds a manual
  `@sveltejs/kit` type import.

## Repair scope

- Add generated `$app/types` to declaration emit only when package sources
  import `$app/server` and the generated types are available.
- Preserve the user config by creating a temporary tsconfig wrapper for the
  declaration emit path.
- Add package fixtures covering a custom library export of a remote query and
  expected `RemoteQueryFunction` output.
- Not claimed: sveltejs/kit#16559 did not merge.
- Not claimed: This record does not claim upstream acceptance of the proposed
  declaration-emitter strategy.

## Validation record

- Public contribution branch was based on SvelteKit's public `version-3`
  branch.
- Latest public pull request head recorded here:
  `84b57ec8f8c2d704b3a867d17bdef4a9965d542b`.
- Public pull request validation recorded:
  `pnpm --dir packages/package test`.
- Public pull request validation recorded:
  `pnpm --dir packages/package check`.
- Public pull request validation recorded:
  `pnpm --dir packages/package lint`.
- Public pull request validation recorded that repo-wide `pnpm lint` and
  `pnpm check` were attempted after rebasing, and both failed on the latest
  `version-3` base because an untouched client runtime file declared an unused
  `warned_on_reset`.

## Public review status

- sveltejs/kit#16559 was opened against `sveltejs/kit:version-3`.
- The pull request was opened from the public `scarab-systems/kit` fork.
- The pull request was linked to sveltejs/kit#14409 in its branch metadata.
- The pull request was closed without merge on 2026-07-29.
- sveltejs/kit#14409 remains open at recording.

## Public links

- https://github.com/sveltejs/kit/issues/14409
- https://github.com/sveltejs/kit/pull/16559

## Changed public files

- .changeset/remote-function-package-types.md
- packages/package/src/filesystem.js
- packages/package/src/index.js
- packages/package/src/typescript.js
- packages/package/test/fixtures/svelte-kit-relative-types/jsconfig.json
- packages/package/test/fixtures/svelte-kit-relative-types/types/index.d.ts
- packages/package/test/fixtures/svelte-kit/expected/index.d.ts
- packages/package/test/fixtures/svelte-kit/expected/index.js
- packages/package/test/fixtures/svelte-kit/expected/remote-query.remote.d.ts
- packages/package/test/fixtures/svelte-kit/expected/remote-query.remote.js
- packages/package/test/fixtures/svelte-kit/package.json
- packages/package/test/fixtures/svelte-kit/src/kitlib/index.js
- packages/package/test/fixtures/svelte-kit/src/kitlib/remote-query.remote.js
- packages/package/test/index.spec.js

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
