---
title: "pnpm #13526"
slug: pnpm-pnpm-13526
repository: pnpm/pnpm
issue_url: https://github.com/pnpm/pnpm/issues/13526
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-08-11
---
# pnpm #13526

## Public case summary

- Repository: `pnpm/pnpm`
- Issue: https://github.com/pnpm/pnpm/issues/13526
- Pull request: https://github.com/pnpm/pnpm/pull/13812
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded
- Upstream status: open pull request recorded on 2026-08-11.

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
  `bd38335a038dbe2467dcef64cf1026b2112fa82e`.
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
- Automated review status visible at recording included CodeRabbit approval and
  Greptile approval.

## Public review status

- pnpm/pnpm#13812 is open against `pnpm/pnpm:main`.
- The pull request was opened from the public `scarab-systems/pnpm` fork.
- The pull request fixes pnpm/pnpm#13526.
- Maintainer edits are enabled.
- pnpm/pnpm#13526 remains open at recording.

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
