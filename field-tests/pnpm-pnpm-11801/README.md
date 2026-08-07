---
title: "pnpm #11801"
slug: pnpm-pnpm-11801
repository: pnpm/pnpm
issue_url: https://github.com/pnpm/pnpm/issues/11801
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-08-07
---
# pnpm #11801

## Public case summary

- Repository: `pnpm/pnpm`
- Issue: https://github.com/pnpm/pnpm/issues/11801
- Pull request: https://github.com/pnpm/pnpm/pull/13671
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: pnpm/pnpm#13671 merged on 2026-08-07.
- Issue status: pnpm/pnpm#11801 closed by the merged pull request.

## Diagnostic finding

- The public issue reported that `pnpm install --offline --frozen-lockfile`
  could still contact the remote registry while verifying the lockfile against
  supply-chain policy metadata.
- Maintainer guidance kept verification in scope but required offline
  verification to use cached metadata instead of remote registry access.
- The useful repair boundary was lockfile resolution verification across the
  TypeScript and Rust resolver stacks.
- Offline verification should read from the local metadata mirror, skip remote
  registry and attestation fallback paths, and report a clear cache-miss error
  when required metadata is unavailable.

## Repair scope

- Thread offline mode into lockfile resolution verification in both the
  TypeScript and Rust resolver paths.
- Make verifier metadata lookups use the local metadata mirror while offline.
- Return `ERR_PNPM_NO_OFFLINE_META` when required metadata is not cached.
- Skip attestation fallback while offline so verification does not reach the
  registry.
- Add Rust and TypeScript coverage for cache hits, cache misses, tarball
  verification, and blocked registry requests.
- Add changeset metadata for resolver and package releases.
- Not claimed: This does not disable supply-chain verification entirely.
- Not claimed: This does not redesign pnpm's lockfile or package-resolution
  model.

## Validation record

- Public contribution branch was based on pnpm's public `main` branch.
- Latest public pull request head before merge:
  `9092ffe200551909ba2131463c1d767c123eae3c`.
- Public pull request validation recorded:
  `cargo test -p pacquet-resolving-npm-resolver --lib`.
- Public pull request validation recorded:
  `cargo test -p pacquet-package-manager build_resolution_verifiers --lib`.
- Public pull request validation recorded:
  `pn --filter @pnpm/resolving.npm-resolver run test`.
- Public pull request validation recorded:
  `pn --filter @pnpm/resolving.default-resolver run test`.
- Public pull request validation recorded `pn run compile-only`,
  `pn run lint --quiet`, and `bash pnpm/scripts/pre-push-rust.sh`.
- Public status checks visible at merge included successful TypeScript CI, Rust
  CI, CodeQL, Code Coverage, dependency audit, benchmark reports, and Greptile
  review; `[code]smith` was skipped.
- CodeRabbit initially requested changes, then approved the revised head.
- pnpm maintainer `zkochan` approved the pull request on 2026-08-07.
- Merge commit: pnpm/pnpm@745924b682ae9d6c3f0857cd65cdabae8fe76a10

## Public review status

- pnpm/pnpm#13671 was opened against `pnpm/pnpm:main`.
- The pull request was opened from the public `scarab-systems/pnpm` fork.
- The pull request fixed pnpm/pnpm#11801.
- The pull request was approved by pnpm maintainer `zkochan`.
- The pull request merged into `pnpm:main` on 2026-08-07.
- The merged pull request closed pnpm/pnpm#11801.

## Public links

- https://github.com/pnpm/pnpm/issues/11801
- https://github.com/pnpm/pnpm/issues/11801#issuecomment-4506154138
- https://github.com/pnpm/pnpm/pull/13671
- https://github.com/pnpm/pnpm/pull/13671#pullrequestreview-4871105381
- https://github.com/pnpm/pnpm/pull/13671#pullrequestreview-4883254256
- https://github.com/pnpm/pnpm/commit/745924b682ae9d6c3f0857cd65cdabae8fe76a10

## Changed public files

- .changeset/offline-lockfile-verification-cache.md
- pnpm/crates/package-manager/src/build_resolution_verifiers.rs
- pnpm/crates/package-manager/src/build_resolution_verifiers/tests.rs
- pnpm/crates/resolving-npm-resolver/src/create_npm_resolution_verifier.rs
- pnpm/crates/resolving-npm-resolver/src/create_npm_resolution_verifier/tests.rs
- pnpm/crates/resolving-npm-resolver/src/errors.rs
- pnpm/crates/resolving-npm-resolver/src/fetch_full_metadata_cached.rs
- pnpm/crates/resolving-npm-resolver/src/fetch_full_metadata_cached/tests.rs
- pnpm/crates/resolving-npm-resolver/src/pick_package.rs
- pnpm11/resolving/default-resolver/src/index.ts
- pnpm11/resolving/npm-resolver/src/createNpmResolutionVerifier.ts
- pnpm11/resolving/npm-resolver/src/fetchFullMetadataCached.ts
- pnpm11/resolving/npm-resolver/test/createNpmResolutionVerifier.test.ts
- pnpm11/resolving/npm-resolver/test/ifModifiedSince.test.ts

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
