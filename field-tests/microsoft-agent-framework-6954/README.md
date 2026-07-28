---
title: "Microsoft Agent Framework #6954"
slug: microsoft-agent-framework-6954
repository: microsoft/agent-framework
issue_url: https://github.com/microsoft/agent-framework/issues/6954
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-07-28
---
# Microsoft Agent Framework #6954

## Public case summary

- Repository: `microsoft/agent-framework`
- Issue: https://github.com/microsoft/agent-framework/issues/6954
- Pull request: https://github.com/microsoft/agent-framework/pull/7322
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: microsoft/agent-framework#7322 merged on 2026-07-28.

## Diagnostic finding

- The public issue reported that Python harness agents using
  `GeminiChatClient` could fail before or during stream startup when the harness
  tool set was combined with Gemini.
- One failure surface was tool schema preparation: Agent Framework
  `FunctionTool` parameter schemas can contain JSON Schema constructs such as
  `$ref`, `$defs`, and `oneOf`, while the Gemini SDK `parameters` field accepts a
  narrower schema subset.
- A second failure surface was Gemini Developer API request preparation: mixed
  native Gemini tools, such as Google Search grounding, and function
  declarations require server-side tool invocation reporting to be enabled.
- The useful repair boundary was the Python Gemini chat client request-prep
  path, not harness assembly or other provider clients.
- Vertex AI behavior and existing function-calling configuration needed to
  remain unchanged.

## Repair scope

- Forward `FunctionTool` schemas through the Gemini SDK
  `parameters_json_schema` field.
- Enable server-side tool invocation reporting only for Gemini Developer API
  requests that mix native Gemini tools with function declarations.
- Preserve existing function-calling configuration, including `tool_choice`.
- Keep Vertex AI behavior unchanged.
- Add Gemini client regression coverage for JSON Schema pass-through,
  mixed-tool Developer API configuration, `tool_choice` preservation, and
  Vertex AI non-impact.
- Add a Python 3.11-compatible test helper import after review surfaced a
  compatibility failure.

## Validation record

- Public pull request head before merge:
  `9587fdfe1638cd600dabadfceaad372ca3ea49cd`.
- Public pull request checks before merge included passing merge gatekeeper,
  pre-commit hooks, Python package checks, Python coverage, Python tests across
  supported Ubuntu and Windows versions, typing checks, samples and Markdown,
  dotnet aggregate build/test gate, Python integration aggregate gate, and
  license/CLA.
- Python coverage automation reported 90% coverage with 9,414 tests, 34 skipped,
  0 failures, and 0 errors.
- Some integration and dotnet shard checks were skipped by the repository's
  path filtering for this Python-only change.
- Public pull request status update: merged on 2026-07-28.
- Merge commit: microsoft/agent-framework@3daa3b2c2deded4472ac170f4cd5da7a89553da4

## Public review status

- microsoft/agent-framework#7322 was opened against
  `microsoft/agent-framework:main`.
- The pull request was opened from the public `scarab-systems/agent-framework`
  fork.
- The pull request fixed microsoft/agent-framework#6954.
- The pull request received approvals from `TaoChenOSU` and
  `eavanvalkenburg`.
- Microsoft Agent Framework maintainer `eavanvalkenburg` merged the pull
  request into `microsoft/agent-framework:main` on 2026-07-28.
- The issue was closed as completed on 2026-07-28 after the pull request
  merged.

## Public links

- https://github.com/microsoft/agent-framework/issues/6954
- https://github.com/microsoft/agent-framework/pull/7322
- https://github.com/microsoft/agent-framework/pull/7322#issuecomment-5105262212
- https://github.com/microsoft/agent-framework/commit/3daa3b2c2deded4472ac170f4cd5da7a89553da4

## Changed public files

- python/packages/gemini/agent_framework_gemini/_chat_client.py
- python/packages/gemini/tests/test_gemini_client.py

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
