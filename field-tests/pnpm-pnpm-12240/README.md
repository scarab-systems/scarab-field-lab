---
title: "pnpm #12240"
slug: pnpm-pnpm-12240
repository: pnpm/pnpm
issue_url: https://github.com/pnpm/pnpm/issues/12240
mode: repair
status: upstream-accepted
recorded_at: 2026-06-10
---
# pnpm #12240

## Public case summary

- Repository: `pnpm/pnpm`
- Issue: https://github.com/pnpm/pnpm/issues/12240
- Pull request: https://github.com/pnpm/pnpm/pull/12301
- Mode: repair
- Status: upstream-accepted

## Diagnostic finding

- The public issue reported that `pnpm self-upgrade` failed with `ERR_PNPM_NO_PKG_MANIFEST` when run from a directory without a `package.json`.
- The repair targeted the dependency-status check path used by `self-upgrade`: a missing manifest caused an intentionally skipped status check to fall through into dependency auto-install.
- Not claimed: This case does not claim a broad redesign of dependency-status checks or global pnpm upgrade behavior.

## Repair scope

- Skip dependency auto-install when dependency status was intentionally skipped because no project manifest was present.
- Preserve the existing fallback for unexpected unknown dependency status when a project manifest exists.
- Add regression coverage for both the no-manifest skip path and the manifest-present fallback path.

## Validation record

- Install: `volta run --node 22.22.2 corepack pnpm install --frozen-lockfile`
- Install result: passed.
- Targeted tests from `exec/commands`: `pnpm exec jest test/runDepsStatusCheck.test.ts test/createInstallArgs.test.ts --runInBand`
- Targeted test result: passed.
- TypeScript build from `exec/commands`: `pnpm exec tsgo --build --pretty false`
- TypeScript build result: passed.
- Package lint from `exec/commands`: `pnpm run lint`
- Package lint result: passed.

## Public review status

- Upstream pull request pnpm/pnpm#12301 was merged on 2026-06-10.
- Merge commit: pnpm/pnpm@2c0b91d6b976c7be9655d472dedecf38c497860e
- The merged pull request closed pnpm/pnpm#12240.

## Public links

- https://github.com/pnpm/pnpm/issues/12240
- https://github.com/pnpm/pnpm/pull/12301
- https://github.com/pnpm/pnpm/pull/12301#issuecomment-4668766700

## Changed public files

- .changeset/deps-status-no-manifest.md
- exec/commands/src/runDepsStatusCheck.ts
- exec/commands/test/runDepsStatusCheck.test.ts

## Record limits

This case publishes public links, findings, validation summaries, and status only.
