---
title: "pnpm #13526"
slug: pnpm-pnpm-13526
repository: pnpm/pnpm
issue_url: https://github.com/pnpm/pnpm/issues/13526
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-08-11
---
# pnpm #13526

## Public case summary

- Repository: `pnpm/pnpm`
- Issue: https://github.com/pnpm/pnpm/issues/13526
- Pull request: https://github.com/pnpm/pnpm/pull/13812
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: pnpm/pnpm#13812 merged on 2026-08-20.
- Issue status: pnpm/pnpm#13526 closed by the merged pull request.

## Diagnostic finding

- The public issue reports that `pacquet update --no-save` can update the
  lockfile with the requested specifier even when `package.json` remains on the
  original in-range dependency range.
- That can make a later frozen install reject the lockfile as out of date even
  though the manifest was intentionally not saved.
- The useful repair boundary was Pacquet's update and fresh-lockfile
  materialization path, not general dependency resolution.
- The update flow still needs the transient requested selector for resolution,
  but lockfile importer specifier serialization needs the kept manifest source
  when `--no-save` is used.

## Repair scope

- Preserve the kept manifest specifier when `pacquet update --no-save` writes
  importer entries into `pnpm-lock.yaml`.
- Keep resolution behavior pointed at the requested selector.
- Pass a separate per-importer manifest source to the fresh-lockfile builder for
  no-save lockfile specifier serialization.
- Add Pacquet regression coverage for no-save lockfile-only updates,
  transformed manifests, and duplicate hook prevention.
- Not claimed: This does not redesign pnpm's package-resolution model.
- Not claimed: This does not change TypeScript CLI behavior where the issue
  discussion already indicates the manifest is not rewritten when `save` is
  false.

## Validation record

- Public contribution branch was based on pnpm's public `main` branch.
- Latest public pull request head recorded here:
  `45f884fab2bcb8d719a07279b4cbe43fa031a495`.
- Public merge commit recorded here:
  `2f5af10931c390b3b7041fa666886af7aad1337a`.
- Public pull request validation recorded:
  `cargo fmt --all --check`.
- Public pull request validation recorded:
  `cargo test -p pacquet-cli update_no_save_keeps_importer_specifier_for_admitted_version --test suite`.
- Public pull request validation recorded that this targeted regression failed
  before the fix and passed after.
- Public pull request validation recorded:
  `cargo test -p pacquet-cli update_no_save --test suite`.
- Public pull request validation recorded:
  `cargo test -p pacquet-cli update_latest_no_save --test suite`.
- Public pull request validation recorded:
  `cargo test -p pacquet-package-manager install_with_drop_all_seed_policy_bumps_dependency_within_range`.
- Public pull request validation recorded:
  `cargo test -p pacquet-cli filtered_update --test suite`.
- Public pull request validation recorded:
  `cargo test -p pacquet-cli recursive_update --test suite`.
- Public pull request validation recorded: `just ready`.
- Public GitHub checks visible before merge included successful Rust CI, CodeQL,
  dependency audit, coverage upload, and benchmark comparison jobs.
- Automated review status visible before merge included CodeRabbit approval and
  Greptile approval.
- Maintainer `zkochan` approved the pull request before merge.

## Public review status

- pnpm/pnpm#13812 merged into `pnpm/pnpm:main` on 2026-08-20.
- The pull request was opened from the public `scarab-systems/pnpm` fork.
- The merged pull request closed pnpm/pnpm#13526.

## Public links

- https://github.com/pnpm/pnpm/issues/13526
- https://github.com/pnpm/pnpm/pull/13812

## Changed public files

- .changeset/keep-no-save-importer-specifier.md
- pnpm/crates/cli/tests/suite/update.rs
- pnpm/crates/package-manager/src/install.rs
- pnpm/crates/package-manager/src/install/materialize.rs
- pnpm/crates/package-manager/src/install/run.rs
- pnpm/crates/package-manager/src/install_with_fresh_lockfile.rs
- pnpm/crates/package-manager/src/update.rs

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
