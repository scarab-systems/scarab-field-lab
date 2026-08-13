---
title: "NemoClaw #7348"
slug: nvidia-nemoclaw-7348
repository: NVIDIA/NemoClaw
issue_url: https://github.com/NVIDIA/NemoClaw/issues/7348
mode: diagnostic-proof-and-repair
status: upstream-closed
recorded_at: 2026-07-25
---
# NemoClaw #7348

## Public case summary

- Repository: `NVIDIA/NemoClaw`
- Issue: https://github.com/NVIDIA/NemoClaw/issues/7348
- Pull request: https://github.com/NVIDIA/NemoClaw/pull/7540
- Mode: diagnostic-proof-and-repair
- Status: upstream-closed
- Upstream status: pull request closed without merge on 2026-07-26.
- Issue status: NVIDIA/NemoClaw#7348 was closed upstream on 2026-07-26.

## Diagnostic finding

- The public issue reports that gateway failure status can be misclassified for
  sandboxes using a non-default `NEMOCLAW_GATEWAY_PORT`.
- The useful repair boundary was the sandbox gateway failure classifier.
- The classifier should resolve the sandbox's owning gateway container and
  gateway port before reporting runtime failures, instead of probing a fixed
  default gateway container and port.

## Repair scope

- Resolve gateway failure checks through the existing sandbox target gateway
  helper.
- Use the resolved gateway port when distinguishing exited-container port
  conflicts.
- Add regression coverage for a non-default gateway sandbox using
  `nemoclaw-8081` and port `8081`.
- Not claimed: NVIDIA/NemoClaw#7540 did not merge.
- Not claimed: This record does not claim final upstream release inclusion for
  the proposed classifier change.

## Validation record

- Public contribution branch was based on NemoClaw's public `main` branch.
- Latest public pull request head recorded here:
  `d31dfd2791576e4b4e272ed25ad6e7664ed6fd74`.
- Public pull request validation recorded: `npm run check:diff`.
- Public pull request validation recorded:
  `npm exec -- vitest run --project integration test/gateway-failure-classifier.test.ts --reporter=dot`.
- The targeted integration run recorded 30 passed tests.
- Public pull request validation recorded that broad `npm test` was attempted
  locally and failed in unrelated installer, onboard, Hermes, runtime, and host
  prerequisite areas outside the touched classifier regression.

## Public review status

- NVIDIA/NemoClaw#7540 was opened against `NVIDIA/NemoClaw:main`.
- The pull request was opened from the public `scarab-systems/NemoClaw` fork.
- The pull request was linked to NVIDIA/NemoClaw#7348.
- The pull request was closed without merge on 2026-07-26.
- NVIDIA/NemoClaw#7348 was closed upstream on 2026-07-26.

## Public links

- https://github.com/NVIDIA/NemoClaw/issues/7348
- https://github.com/NVIDIA/NemoClaw/pull/7540

## Changed public files

- src/lib/actions/sandbox/gateway-failure-classifier.ts
- test/gateway-failure-classifier.test.ts

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
