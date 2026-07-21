---
title: "Microsoft Agent Framework #7160"
slug: microsoft-agent-framework-7160
repository: microsoft/agent-framework
issue_url: https://github.com/microsoft/agent-framework/issues/7160
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-07-21
---
# Microsoft Agent Framework #7160

## Public case summary

- Repository: `microsoft/agent-framework`
- Issue: https://github.com/microsoft/agent-framework/issues/7160
- Pull request: https://github.com/microsoft/agent-framework/pull/7189
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: microsoft/agent-framework#7189 merged on 2026-07-21.

## Diagnostic finding

- The public issue reported that Python MCP sampling could forward tool schemas
  to the configured chat client, but could not convert Agent Framework
  `function_call` content back into an MCP structured sampling response.
- The visible failure was an internal content-type error even when the model had
  returned a structured function call matching the supplied output-schema tool.
- Plain text and image sampling were not the failing surface. The useful repair
  boundary was the conversion point between Agent Framework response content and
  MCP sampling result content.
- Structured sampling needed function-call contents to become MCP
  `ToolUseContent`, with a `toolUse` stop reason when tool calls are present.

## Repair scope

- Convert Agent Framework `function_call` content into MCP `ToolUseContent`.
- Preserve each function call's ID, name, and arguments.
- Collect prepared content across all response messages so multiple tool-use
  calls are preserved.
- Return `CreateMessageResultWithTools` with `stopReason="toolUse"` when
  tool-use content is present.
- Keep existing text/image sampling behavior and existing approval, rate, token,
  and fallback handling intact.
- Add regression coverage for structured sampling output with multiple tool-use
  calls.

## Validation record

- Public pull request head before merge:
  `5a87dd69609caa2063415f61e4d7a9fcbff4f7d3`.
- Public pull request checks before merge included passing merge gatekeeper,
  pre-commit hooks, Python package checks, Python coverage, Python tests across
  supported Ubuntu and Windows versions, typing checks, samples and Markdown,
  dotnet aggregate build/test gate, Python integration aggregate gate, and
  license/CLA.
- Some integration and dotnet shard checks were skipped by the repository's
  path filtering for this Python-only change.
- Public pull request status update: merged on 2026-07-21.
- Merge commit: microsoft/agent-framework@9e836f7b42541a1e3b1ec1208b3f1667ffd4d645

## Public review status

- The pull request was opened from the public
  `scarab-systems/agent-framework-7160-mcp-sampling` branch.
- The pull request linked and closed microsoft/agent-framework#7160.
- The pull request received maintainer approvals from `moonbox3` and
  `eavanvalkenburg`.
- Microsoft Agent Framework maintainer `eavanvalkenburg` merged the pull request
  into `microsoft/agent-framework:main` on 2026-07-21.
- The issue was closed as completed on 2026-07-21 after the pull request merged.

## Public links

- https://github.com/microsoft/agent-framework/issues/7160
- https://github.com/microsoft/agent-framework/pull/7189
- https://github.com/microsoft/agent-framework/pull/7189#event-28268244487
- https://github.com/microsoft/agent-framework/commit/9e836f7b42541a1e3b1ec1208b3f1667ffd4d645

## Changed public files

- python/packages/core/agent_framework/_mcp.py
- python/packages/core/tests/core/test_mcp.py

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
