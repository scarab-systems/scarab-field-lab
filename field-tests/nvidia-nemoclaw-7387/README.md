---
title: "NemoClaw #7387"
slug: nvidia-nemoclaw-7387
repository: NVIDIA/NemoClaw
issue_url: https://github.com/NVIDIA/NemoClaw/issues/7387
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-07-25
---
# NemoClaw #7387

## Public case summary

- Repository: `NVIDIA/NemoClaw`
- Issue: https://github.com/NVIDIA/NemoClaw/issues/7387
- Pull request: https://github.com/NVIDIA/NemoClaw/pull/7406
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: NVIDIA/NemoClaw#7406 merged on 2026-07-25.

## Diagnostic finding

- The public issue reports that `doctor` should detect sandbox registry entries
  that are incomplete for snapshot and rebuild flows.
- The visible risk was a registered sandbox appearing readable while lifecycle
  metadata needed by later snapshot, rebuild, recovery, or related operations
  was missing or invalid.
- The useful repair boundary was the read-only sandbox doctor lifecycle
  registration contract, not a live recovery or credentialed Brev operation.
- A diagnostic check should report missing or invalid metadata field names
  without returning registry values or captured runtime output.
- Invalid registered gateway bindings should be reported rather than probed or
  recovered against the wrong gateway.

## Repair scope

- Add a read-only doctor check for registered sandbox lifecycle metadata.
- Validate lifecycle fields, image provenance, durable dashboard/gateway port
  requirements, and registered gateway binding readiness.
- Warn when lifecycle metadata is incomplete or invalid, including affected
  operations and re-registration guidance.
- Report field names, not stored registry values or captured runtime output.
- Update doctor command documentation for lifecycle-readiness behavior.
- Add lifecycle-registration and doctor-flow regression coverage.
- Not claimed: No credentialed or live Brev environment was used for the pull
  request validation.

## Validation record

- Public contribution branch was based on NemoClaw's public `main` branch.
- Latest public pull request head before merge:
  `6bc39b72ad0bb7dc96069c5d164485e5a793cd63`.
- Public pull request validation recorded passing CLI build and type-check:
  `npm run build:cli` and `npm run typecheck:cli`.
- Focused lifecycle-registration and doctor-flow tests passed:
  `npx vitest run --project cli src/lib/actions/sandbox/doctor-lifecycle-registration.test.ts src/lib/actions/sandbox/doctor-flow.test.ts`.
- Additional pull request validation recorded passing `npm run test:changed`,
  `npm run check:diff`, and `npm run docs`.
- Public review automation recorded 0 actionable findings in the canonical
  review ledger.
- Public pull request status update: merged into `NVIDIA/NemoClaw:main` on
  2026-07-25.
- Merge commit: NVIDIA/NemoClaw@f69598075495366a4587ee6d0556383cbba74392

## Public review status

- NVIDIA/NemoClaw#7406 was opened against `NVIDIA/NemoClaw:main`.
- The pull request was opened from the public `scarab-systems/NemoClaw` fork.
- The pull request fixed NVIDIA/NemoClaw#7387.
- Maintainer `prekshivyas` approved and merged the pull request on 2026-07-25.
- The merged pull request closed NVIDIA/NemoClaw#7387 as completed.

## Public links

- https://github.com/NVIDIA/NemoClaw/issues/7387
- https://github.com/NVIDIA/NemoClaw/pull/7406
- https://github.com/NVIDIA/NemoClaw/pull/7406#issuecomment-5053102021
- https://github.com/NVIDIA/NemoClaw/pull/7406#event-28471659599
- https://github.com/NVIDIA/NemoClaw/commit/f69598075495366a4587ee6d0556383cbba74392

## Changed public files

- docs/reference/commands.mdx
- src/lib/actions/sandbox/doctor-flow.test.ts
- src/lib/actions/sandbox/doctor-lifecycle-registration.test.ts
- src/lib/actions/sandbox/doctor-lifecycle-registration.ts
- src/lib/actions/sandbox/doctor.ts
- src/lib/domain/lifecycle-registration.ts
- test/cli/doctor-gateway-token.test.ts
- test/cli/helpers.ts

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
