---
title: "NemoClaw #6042"
slug: nvidia-nemoclaw-6042
repository: NVIDIA/NemoClaw
issue_url: https://github.com/NVIDIA/NemoClaw/issues/6042
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-21
---
# NemoClaw #6042

## Public case summary

- Repository: `NVIDIA/NemoClaw`
- Issue: https://github.com/NVIDIA/NemoClaw/issues/6042
- Pull request: https://github.com/NVIDIA/NemoClaw/pull/7254
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

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

## Repair scope

- Require a non-empty original recorded policy preset selection before the
  policies step can be skipped on resume.
- Add regression coverage for an empty recorded policy preset list, proving that
  policy setup runs instead of recording a skipped state.
- Add regression coverage for an empty recorded preset list that expands into a
  required preset before the already-applied check.
- Not claimed: NVIDIA/NemoClaw#7254 has not merged at recording.
- Not claimed: This record does not claim full live onboarding E2E coverage for
  every macOS resume path.

## Validation record

- Public contribution branch was based on NemoClaw's public `main` branch.
- Latest public pull request head recorded here:
  `7f8548d02ea9880a6e2df9d59b3b96479c003c81`.
- Targeted behavior tests passed:
  `npx vitest run --project cli src/lib/onboard/machine/handlers/policies.test.ts src/lib/onboard/machine/handlers/policies-restricted-resume.test.ts src/lib/onboard/policy-resume-selection.test.ts src/lib/onboard/policy-preset-persistence.test.ts`
  passed with 44 tests.
- Difference checks passed: `npm run check:diff`.
- Public checks visible at recording included passing CodeRabbit,
  codebase-growth-guardrails, require-maintainer-edits, and PR Review Advisor
  jobs.
- PR Review Advisor status at recording: informational, with 0 blockers and 1
  non-blocking warning requesting broader live onboarding-resume E2E coverage.
- The E2E / PR Gate job timed out waiting for NVIDIA's trusted E2E verdict. The
  visible log did not show a direct patch failure.

## Public review status

- NVIDIA/NemoClaw#7254 is open against `NVIDIA/NemoClaw:main`.
- The pull request was opened from the public `scarab-systems/NemoClaw` fork.
- The pull request is related to NVIDIA/NemoClaw#6042.
- Public status at recording: open, not draft, and not merged.
- GitHub review status at recording: review required.
- GitHub merge-state field at recording: blocked.
- Maintainer edits were enabled at recording.
- A public contributor comment states that the fix addresses the empty
  `policyPresets` resume edge case.

## Public links

- https://github.com/NVIDIA/NemoClaw/issues/6042
- https://github.com/NVIDIA/NemoClaw/pull/7254
- https://github.com/NVIDIA/NemoClaw/pull/7254#issuecomment-5026661363

## Changed public files

- src/lib/onboard/machine/handlers/policies.ts
- src/lib/onboard/machine/handlers/policies.test.ts

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
