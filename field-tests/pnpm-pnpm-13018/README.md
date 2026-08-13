---
title: "pnpm #13018"
slug: pnpm-pnpm-13018
repository: pnpm/pnpm
issue_url: https://github.com/pnpm/pnpm/issues/13018
mode: diagnostic-proof-and-repair
status: upstream-closed
recorded_at: 2026-08-08
---
# pnpm #13018

## Public case summary

- Repository: `pnpm/pnpm`
- Issue: https://github.com/pnpm/pnpm/issues/13018
- Pull request: https://github.com/pnpm/pnpm/pull/13722
- Mode: diagnostic-proof-and-repair
- Status: upstream-closed
- Upstream status: pull request closed without merge on 2026-08-08.
- Issue status: pnpm/pnpm#13018 was closed upstream on 2026-08-08.

## Diagnostic finding

- The public issue reports `MODULE_NOT_FOUND` when Corepack tries to use
  `pnpm@next-12`.
- Corepack maps pnpm packages to `bin/pnpm.mjs` and `bin/pnpx.mjs`, while the
  Pacquet-backed package relied on install-time relinking of wrapper files.
- Corepack does not run that install step before invoking pnpm, so the useful
  repair boundary was the package entrypoint and generated wrapper package
  surface, not the package manager resolver.

## Repair scope

- Add Corepack-compatible `bin/pnpm.mjs` and `bin/pnpx.mjs` entrypoints to the
  pnpm v12 package.
- Resolve the installed native optional dependency package and delegate to its
  native binary without relying on `preinstall`.
- Include the entrypoints in the generated `@pnpm/exe` wrapper package.
- Add package-level regression coverage for wrapper generation and native
  binary delegation.
- Not claimed: pnpm/pnpm#13722 did not merge.
- Not claimed: This record does not claim release inclusion for the proposed
  entrypoint change.

## Validation record

- Public contribution branch was based on pnpm's public `main` branch.
- Latest public pull request head recorded here:
  `d546fdf52519073dad5a178f2c50f9932f076ad8`.
- Public pull request validation recorded:
  `node --test pnpm/npm/pnpm/test/*.test.mjs`.
- Public pull request validation recorded:
  `node --check pnpm/npm/pnpm/bin/pnpm.mjs`.
- Public pull request validation recorded:
  `node --check pnpm/npm/pnpm/bin/pnpx.mjs`.
- Public pull request validation recorded:
  `node --check pnpm/npm/pnpm/scripts/generate-packages.mjs`.
- Public pull request validation recorded:
  `node --check pnpm/npm/pnpm/test/corepack-shim.test.mjs`.
- Public pull request validation recorded:
  `pnpm --config.manage-package-manager-versions=false lint:meta`.
- Public pull request validation recorded:
  `pnpm --config.manage-package-manager-versions=false spellcheck`.
- Public pull request validation recorded:
  `typos .changeset/corepack-pacquet-shim.md pnpm/npm/pnpm`.
- Public pull request validation recorded:
  `npm pack --dry-run --json`.
- Public pull request validation recorded:
  `pnpm --config.manage-package-manager-versions=false pack --dry-run --json`.
- Public pull request validation recorded: `just test-pacquet`.
- Public pull request validation recorded: `just ready`.
- Automated review status visible at recording included CodeRabbit approval.

## Public review status

- pnpm/pnpm#13722 was opened against `pnpm/pnpm:main`.
- The pull request was opened from the public `scarab-systems/pnpm` fork.
- The pull request was linked to pnpm/pnpm#13018.
- The pull request was closed without merge on 2026-08-08.
- pnpm/pnpm#13018 was closed upstream on 2026-08-08.

## Public links

- https://github.com/pnpm/pnpm/issues/13018
- https://github.com/pnpm/pnpm/pull/13722

## Changed public files

- .changeset/corepack-pacquet-shim.md
- pnpm/npm/pnpm/bin/pnpm.mjs
- pnpm/npm/pnpm/bin/pnpx.mjs
- pnpm/npm/pnpm/package.json
- pnpm/npm/pnpm/scripts/generate-packages.mjs
- pnpm/npm/pnpm/test/corepack-shim.test.mjs

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
