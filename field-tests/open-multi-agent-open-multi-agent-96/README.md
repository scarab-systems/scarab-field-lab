---
title: "Open Multi Agent #96"
slug: open-multi-agent-open-multi-agent-96
repository: open-multi-agent/open-multi-agent
issue_url: https://github.com/open-multi-agent/open-multi-agent/issues/96
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-15
---
# Open Multi Agent #96

## Public case summary

- Repository: `open-multi-agent/open-multi-agent`
- Issue: https://github.com/open-multi-agent/open-multi-agent/issues/96
- Pull request: https://github.com/open-multi-agent/open-multi-agent/pull/377
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue proposes per-call risk gating for built-in shell and
  filesystem tools.
- The issue is publicly labeled `enhancement`, `source:feedback`, and `P2`;
  the `P2` label describes community contributions as welcome.
- The repair area centered on the tool execution boundary after validated tool
  input is available and before the tool implementation runs.
- Open Multi Agent already had tool-name access controls through presets,
  allowlists, and denylists, but once a broad tool such as `bash` was granted,
  the framework did not expose a call-level policy hook for that particular
  invocation.
- The useful boundary was therefore not "which tool names are reachable"; it
  was "should this validated invocation run right now?"

## Repair scope

- Add typed public surfaces for a per-call tool gate:
  `ToolCallContext`, `ToolCallDecision`, `ToolCallGate`, and
  `ToolCallGateMetadata`.
- Add optional `onToolCall` configuration to agent and orchestrator paths.
- Run the gate inside `ToolExecutor` after Zod input validation and before
  `tool.execute()`.
- Return a standard error `ToolResult` when the gate denies a call, without
  invoking the tool implementation.
- Preserve existing behavior for ungranted or disallowed tools: those are still
  blocked before the gate runs.
- Add compact trace metadata for evaluated tool calls:
  `gated`, `gateAction`, and `gateReason`.
- Re-export the new public types from the package entry point.
- Add regression coverage for direct executor denial, trace metadata on a
  denied call, and orchestrator-level gating on a granted built-in tool.
- Not claimed: This does not add the optional shell-risk classifier discussed in
  the issue.
- Not claimed: This does not add a general policy engine, sandbox, or isolation
  layer.
- Not claimed: open-multi-agent/open-multi-agent#377 has not merged at
  recording.

## Validation record

- Public contribution branch was based on Open Multi Agent's public `main`
  branch.
- Latest public pull request head recorded here:
  `65fe8ddd1fddeb4ee709a0b46e0d5cfb0a036955`.
- Required contributor-guide checks passed:
  `npm run lint` and `npm test`.
- Additional visible CI-style checks passed:
  `npm run build`, `npm run test:coverage`, and `npm run test:scaffold`.
- Package smoke checks passed for built entry-point imports, CLI help, template
  typechecking, and tarball file lists.
- Public pull request status at recording: open and not draft.
- Maintainer edits were enabled at recording.
- GitHub checks had not yet reported on the pull request branch at recording.
- Not claimed: This record does not claim upstream review, CI completion, or
  merge.

## Public review status

- open-multi-agent/open-multi-agent#377 is open against
  `open-multi-agent/open-multi-agent:main`.
- The pull request was opened from the public `scarab-systems/open-multi-agent`
  fork.
- The pull request is linked as closing open-multi-agent/open-multi-agent#96.
- GitHub's merge-state field showed blocked while review and checks were
  pending at recording.

## Public links

- https://github.com/open-multi-agent/open-multi-agent/issues/96
- https://github.com/open-multi-agent/open-multi-agent/pull/377

## Changed public files

- packages/core/src/agent/agent.ts
- packages/core/src/agent/runner.ts
- packages/core/src/index.ts
- packages/core/src/orchestrator/orchestrator.ts
- packages/core/src/tool/executor.ts
- packages/core/src/types.ts
- packages/core/tests/default-deny-tools.test.ts
- packages/core/tests/tool-executor.test.ts
- packages/core/tests/trace.test.ts

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
