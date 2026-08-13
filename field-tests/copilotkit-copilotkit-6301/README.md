---
title: "CopilotKit #6301"
slug: copilotkit-copilotkit-6301
repository: CopilotKit/CopilotKit
issue_url: https://github.com/CopilotKit/CopilotKit/issues/6301
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-08-08
---
# CopilotKit #6301

## Public case summary

- Repository: `CopilotKit/CopilotKit`
- Issue: https://github.com/CopilotKit/CopilotKit/issues/6301
- Pull request: https://github.com/CopilotKit/CopilotKit/pull/6439
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded
- Upstream status: open pull request recorded on 2026-08-08.

## Diagnostic finding

- The public issue reports a v2 message-view freeze during long multi-tool
  runs where state updates continue but frontend rendering and tool execution
  stop advancing.
- The useful repair boundary was the core run-handler reconciliation path
  between streamed messages and the final authoritative agent message state.
- A late final snapshot can omit assistant or tool messages already observed in
  the stream, leaving `runAgentResult.newMessages` empty even though streamed
  frontend tool-call messages were already seen.
- The repair should preserve only messages created during the current run that
  were observed in the stream and missing from final state, instead of broadly
  replaying historical messages.

## Repair scope

- Track messages observed through `onMessagesChanged` during a single run.
- Compare streamed run messages against the final message state.
- Restore streamed messages from the current run when a late snapshot omits
  them.
- Append only those missing run messages to `newMessages` so frontend tool
  execution receives the streamed tool-call message.
- Add regression coverage for a late truncated snapshot.
- Not claimed: This does not change the external LangGraph protocol.
- Not claimed: This does not replay all existing messages through tool
  execution.

## Validation record

- Public contribution branch was based on CopilotKit's public `main` branch.
- Latest public pull request head recorded here:
  `e609517d536b8bafcb7330dc1e4cab04d1e1d529`.
- Public pull request validation recorded:
  `./node_modules/.bin/vitest run packages/core/src/__tests__/core-run-agent-message-reconciliation.test.ts`.
- Public pull request validation recorded a fail-first check by temporarily
  removing the production reconciliation change and confirming the regression
  test failed because the frontend tool handler was not called.
- Public pull request validation recorded:
  `./node_modules/.bin/vitest run packages/core/src/__tests__`.
- Public pull request validation recorded:
  `./node_modules/.bin/tsc -p packages/core/tsconfig.json --noEmit`.
- Public pull request validation recorded:
  `./node_modules/.bin/oxlint packages/core/src/core/run-handler.ts packages/core/src/__tests__/core-run-agent-message-reconciliation.test.ts`.
- Public pull request validation recorded:
  `./node_modules/.bin/oxfmt --check packages/core/src/core/run-handler.ts packages/core/src/__tests__/core-run-agent-message-reconciliation.test.ts`.
- Public pull request validation recorded:
  `NODE_OPTIONS=--max-old-space-size=8192 ./node_modules/.bin/tsdown`
  from `packages/core`.
- Public pull request validation recorded local hook validation through
  `pnpm exec lefthook validate`, `commit-msg`, and `pre-commit`.

## Public review status

- CopilotKit/CopilotKit#6439 is open against `CopilotKit/CopilotKit:main`.
- The pull request was opened from the public `scarab-systems/CopilotKit` fork.
- The pull request fixes CopilotKit/CopilotKit#6301.
- Maintainer edits are enabled.
- CopilotKit/CopilotKit#6301 remains open at recording.

## Public links

- https://github.com/CopilotKit/CopilotKit/issues/6301
- https://github.com/CopilotKit/CopilotKit/pull/6439

## Changed public files

- packages/core/src/__tests__/core-run-agent-message-reconciliation.test.ts
- packages/core/src/core/run-handler.ts

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
