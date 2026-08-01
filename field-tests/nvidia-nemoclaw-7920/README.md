---
title: "NemoClaw #7920"
slug: nvidia-nemoclaw-7920
repository: NVIDIA/NemoClaw
issue_url: https://github.com/NVIDIA/NemoClaw/issues/7920
mode: diagnostic-proof-and-repair
status: upstream-closed
recorded_at: 2026-07-31
---
# NemoClaw #7920

## Public case summary

- Repository: `NVIDIA/NemoClaw`
- Issue: https://github.com/NVIDIA/NemoClaw/issues/7920
- Related epic: https://github.com/NVIDIA/NemoClaw/issues/7912
- Follow-up issue: https://github.com/NVIDIA/NemoClaw/issues/7915
- Pull request: https://github.com/NVIDIA/NemoClaw/pull/7956
- Mode: diagnostic-proof-and-repair
- Status: upstream-closed
- Upstream status: NVIDIA/NemoClaw#7956 closed without merge on 2026-07-31.
- Issue status: NVIDIA/NemoClaw#7920 closed on 2026-07-31 after the
  maintainer scheduling decision changed.

## Diagnostic finding

- The public issue requested execution tiers for retained OpenShell gateway
  compatibility rows so ordinary scheduled runs could reduce runner cost while
  preserving historical and architecture coverage for weekly, release, and
  explicit maintainer-dispatch paths.
- The pull request implemented the issue scope by documenting retained gateway
  row boundaries and tiers, selecting rows by tier, reporting skipped-by-tier
  rows, and adding workflow-boundary tests.
- Maintainer review later rejected the weekly-schedule premise: retained
  compatibility boundaries that still matter must run for every release
  candidate, because weekly execution can miss a release-time regression.
- The useful follow-up boundary is optimization of retained live coverage, not
  deferred execution of required coverage beyond the release cadence.

## Repair scope

- Add `execution_tier` and `unique_boundary` metadata for retained gateway
  upgrade rows.
- Keep the current canonical migration row in the ordinary nightly tier.
- Keep historical and architecture rows selectable for explicit dispatch,
  weekly coverage, and release qualification as the issue originally requested.
- Gate preparation, live execution, and artifact upload through the row
  classifier so skipped rows are reported clearly.
- Add workflow-boundary coverage for tier metadata, classification, caller
  gates, forced selection, and skipped-row reporting.
- Not claimed: NVIDIA/NemoClaw#7956 did not merge.
- Not claimed: The maintainer closure was not an implementation-quality
  acceptance or rejection.
- Not claimed: This record does not claim that weekly retained compatibility
  coverage remains the correct direction for NemoClaw.

## Validation record

- Public contribution branch was based on NemoClaw's public `main` branch.
- Latest public pull request head recorded here:
  `46dda8ce5f87508da795f71e52f6c49f71c7758b`.
- Public pull request validation recorded `npm run validate:pr` passing for
  `032e419aa`.
- Targeted workflow-boundary validation recorded:
  `npx vitest run --project e2e-support test/e2e/support/openshell-gateway-upgrade-workflow-boundary.test.ts`.
- Additional pull request validation recorded passing focused
  workflow/report/planner checks, `npm run test:changed`, and
  `npm run test:e2e-phases:check`.
- Public PR Review Advisor status reported no blocking findings.
- Public maintainer closure stated that NVIDIA/NemoClaw#7956 implemented
  NVIDIA/NemoClaw#7920 as written, and that the scheduling premise, not
  implementation quality, was the reason for closure.
- Follow-up optimization remains tracked in NVIDIA/NemoClaw#7912, with
  exact-commit artifact reuse tracked in NVIDIA/NemoClaw#7915.

## Public review status

- NVIDIA/NemoClaw#7956 was opened against `NVIDIA/NemoClaw:main`.
- The pull request was opened from the public `scarab-systems/NemoClaw` fork.
- The pull request was related to NVIDIA/NemoClaw#7920.
- Public status at recording: closed without merge.
- NVIDIA/NemoClaw#7920 was closed as not planned after the scheduling decision
  changed.
- No upstream merge or acceptance is claimed for this case.

## Public links

- https://github.com/NVIDIA/NemoClaw/issues/7920
- https://github.com/NVIDIA/NemoClaw/issues/7920#issuecomment-5147983673
- https://github.com/NVIDIA/NemoClaw/issues/7912
- https://github.com/NVIDIA/NemoClaw/issues/7915
- https://github.com/NVIDIA/NemoClaw/pull/7956
- https://github.com/NVIDIA/NemoClaw/pull/7956#issuecomment-5147985438
- https://github.com/NVIDIA/NemoClaw/pull/7956#issuecomment-5148164095

## Changed public files

- .github/workflows/e2e.yaml
- test/e2e/support/openshell-gateway-upgrade-workflow-boundary.test.ts
- tools/e2e/openshell-gateway-upgrade-workflow-boundary.mts
- tools/e2e/prepare-e2e-workflow-boundary.mts
- tools/e2e/upload-e2e-artifacts-workflow-boundary.mts

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
