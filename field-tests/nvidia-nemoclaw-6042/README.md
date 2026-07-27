---
title: "NemoClaw #6042"
slug: nvidia-nemoclaw-6042
repository: NVIDIA/NemoClaw
issue_url: https://github.com/NVIDIA/NemoClaw/issues/6042
mode: diagnostic-proof
status: upstream-accepted
recorded_at: 2026-07-21
---
# NemoClaw #6042

## Public case summary

- Repository: `NVIDIA/NemoClaw`
- Issue: https://github.com/NVIDIA/NemoClaw/issues/6042
- Pull request: https://github.com/NVIDIA/NemoClaw/pull/7254
- Mode: diagnostic-proof
- Status: upstream-accepted
- Upstream status: NVIDIA/NemoClaw#7254 merged on 2026-07-25.
- Release record: NVIDIA's NemoClaw v0.0.96 announcement on 2026-07-27
  references this contribution and thanks `@scarab-systems`.

## Diagnostic finding

- The public issue reports that the macOS interactive onboarding wizard skips
  the Policy Presets TUI step.
- The visible failure was an onboarding UI step being skipped.
- The useful repair boundary was the onboarding resume state-machine contract,
  not the TUI rendering layer.
- A saved session with `policyPresets: []` could be treated as already having a
  completed policy selection.
- An empty recorded preset list means no policy preset was recorded. It must not
  permit the policies step to take the resume-skip path.

## Contribution scope

- Add regression coverage for the existing empty policy-preset resume contract.
- Exercise the real `arePolicyPresetsApplied` behavior for an empty recorded
  selection.
- Exercise the real policy resume-selection contract when Slack becomes
  required after an empty recorded selection.
- The accepted pull request was test-only; production behavior was unchanged.
- Not claimed: This record does not claim that NVIDIA/NemoClaw#6042 is closed.
- Not claimed: This record does not claim full live onboarding E2E coverage for
  every macOS resume path.

## Validation record

- Public contribution branch was based on NemoClaw's public `main` branch.
- Latest public pull request head before merge:
  `47d3546ce5448a55b86c809c186175f64826304c`.
- Targeted behavior tests passed:
  `npx vitest run --project cli src/lib/onboard/policy-resume-selection.test.ts`
  passed with 13 tests.
- Integration coverage passed:
  `npx vitest run --project integration test/onboard.test.ts` passed with 34
  tests.
- Difference checks passed: `npm run check:diff`.
- Public review automation recorded 0 blockers, 0 warnings, and 0 suggestions.
- Public pull request status update: merged into `NVIDIA/NemoClaw:main` on
  2026-07-25.
- Merge commit: NVIDIA/NemoClaw@ee7a7ebc4d8f5a1343828c093f6d0d6a103add6e
- Public release announcement record: NemoClaw v0.0.96 references
  NVIDIA/NemoClaw#7254 as empty-policy resume protection and thanks
  `@scarab-systems` for the accepted contribution.

## Public review status

- NVIDIA/NemoClaw#7254 was opened against `NVIDIA/NemoClaw:main`.
- The pull request was opened from the public `scarab-systems/NemoClaw` fork.
- The pull request is related to NVIDIA/NemoClaw#6042.
- Maintainer `prekshivyas` approved and merged the pull request on 2026-07-25.
- NVIDIA/NemoClaw#6042 remained open at this status refresh.
- A public contributor comment states that the contribution addresses the empty
  `policyPresets` resume edge case with regression coverage.

## Public links

- https://github.com/NVIDIA/NemoClaw/issues/6042
- https://github.com/NVIDIA/NemoClaw/pull/7254
- https://github.com/NVIDIA/NemoClaw/pull/7254#issuecomment-5026661363
- https://github.com/NVIDIA/NemoClaw/pull/7254#event-28473433505
- https://github.com/NVIDIA/NemoClaw/commit/ee7a7ebc4d8f5a1343828c093f6d0d6a103add6e
- https://github.com/NVIDIA/NemoClaw/discussions/7656

## Changed public files

- src/lib/onboard/policy-resume-selection.test.ts
- test/onboard.test.ts

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
