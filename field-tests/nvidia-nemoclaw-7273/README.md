---
title: "NemoClaw #7273"
slug: nvidia-nemoclaw-7273
repository: NVIDIA/NemoClaw
issue_url: https://github.com/NVIDIA/NemoClaw/issues/7273
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-21
---
# NemoClaw #7273

## Public case summary

- Repository: `NVIDIA/NemoClaw`
- Issue: https://github.com/NVIDIA/NemoClaw/issues/7273
- Pull request: https://github.com/NVIDIA/NemoClaw/pull/7291
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reports that sandbox recovery can falsely fail while the
  recreated gateway is healthy.
- The visible failure was host-forward recovery being withheld after a sandbox
  relaunch.
- The useful repair boundary was the managed supervisor relaunch readiness
  contract across supervisor health, recreated sandbox readiness, OpenShell
  readiness, and host forward recovery.
- The managed-health probe has a tri-state contract: `true` means healthy,
  `false` means definitive failure, and `null` means inconclusive or retryable.
- A relaunch path collapsed the tri-state result into a boolean too early. An
  exact `SUPERVISOR_BUSY` result could become `false` instead of reaching the
  readiness loop as retryable `null`.

## Repair scope

- Preserve `boolean | null` through the recreated-sandbox readiness guard.
- Retry an inconclusive managed-health guard result within the existing
  readiness deadline.
- Fail closed on definitive managed-health failures and unexpected OpenShell
  readiness errors.
- Report managed-health failure, managed-health inconclusive timeout, and
  OpenShell readiness failure as separate forward-recovery details.
- Keep host-forward callbacks fail-closed by coercing the tri-state result only
  at the forward-start boundary.
- Add regression coverage for the supervisor-relaunch caller path where a busy
  pinned managed probe is retried before the replacement forward starts.
- Not claimed: NVIDIA/NemoClaw#7291 has not merged at recording.
- Not claimed: This record does not claim that NVIDIA's credentialed E2E gate
  has completed.

## Validation record

- Public contribution branch was based on NemoClaw's public `main` branch.
- Latest public pull request head recorded here:
  `cf279250b1ba2d531342d9f246148edf8e7985a5`.
- Dependency install recorded by the pull request: `npm ci`.
- Targeted tests recorded by the pull request:
  `npx vitest run --project cli src/lib/actions/sandbox/process-recovery.test.ts test/process-recovery-supervisor-relaunch.test.ts`.
- Integration regression test recorded by the pull request:
  `npx vitest run --project integration test/process-recovery-supervisor-relaunch.test.ts`.
- Formatting and lint checks recorded by the pull request:
  `npx @biomejs/biome format src/lib/actions/sandbox/process-recovery.ts src/lib/actions/sandbox/process-recovery.test.ts test/process-recovery-supervisor-relaunch.test.ts`
  and
  `npx @biomejs/biome lint src/lib/actions/sandbox/process-recovery.ts src/lib/actions/sandbox/process-recovery.test.ts test/process-recovery-supervisor-relaunch.test.ts`.
- TypeScript and build checks recorded by the pull request:
  `npm run typecheck:cli` and `npm run build:cli`.
- Whitespace hygiene passed: `git diff --check`.
- Public checks visible at recording included passing CodeRabbit,
  codebase-growth-guardrails, require-maintainer-edits, and PR Review Advisor
  jobs.
- PR Review Advisor status at recording: informational, with 0 blockers, 0
  warnings, and 0 suggestions.
- CodeRabbit's recent review at recording reported no actionable comments.

## Public review status

- NVIDIA/NemoClaw#7291 is open against `NVIDIA/NemoClaw:main`.
- The pull request was opened from the public `scarab-systems/NemoClaw` fork.
- The pull request is related to NVIDIA/NemoClaw#7273.
- Public status at recording: open, not draft, and not merged.
- GitHub review status at recording: review required.
- GitHub merge-state field at recording: blocked.
- Maintainer edits were enabled at recording.
- NVIDIA-side E2E gate coordination was still in progress at recording.

## Public links

- https://github.com/NVIDIA/NemoClaw/issues/7273
- https://github.com/NVIDIA/NemoClaw/pull/7291
- https://github.com/NVIDIA/NemoClaw/pull/7291#issuecomment-5031054154

## Changed public files

- src/lib/actions/sandbox/process-recovery.ts
- src/lib/actions/sandbox/process-recovery.test.ts
- test/process-recovery-supervisor-relaunch.test.ts

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
