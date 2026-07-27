---
title: "Microsoft Agent Framework #7045"
slug: microsoft-agent-framework-7045
repository: microsoft/agent-framework
issue_url: https://github.com/microsoft/agent-framework/issues/7045
mode: diagnostic-proof-and-repair
status: upstream-closed
recorded_at: 2026-07-27
---
# Microsoft Agent Framework #7045

## Public case summary

- Repository: `microsoft/agent-framework`
- Issue: https://github.com/microsoft/agent-framework/issues/7045
- Pull request: https://github.com/microsoft/agent-framework/pull/7334
- Maintainer consolidation PR: https://github.com/microsoft/agent-framework/pull/7345
- Mode: diagnostic-proof-and-repair
- Status: upstream-closed

## Diagnostic finding

- The public issue reports a Python Agent Framework streaming orphan: after
  `max_function_calls` is reached, a provider can still stream a function call
  even though the framework will not execute that call.
- For transports such as AG-UI, forwarding that over-limit call can expose a
  tool-call start/arguments/end sequence without a matching tool result.
- The useful repair boundary was the Python function invocation streaming loop
  and its invocation-limit fallback handling.
- The failure was not in the AG-UI client, tool implementation, or provider
  execution itself. The framework needed to avoid exposing a post-limit call it
  would not execute.

## Repair scope

- Drop post-limit `function_call` content from streaming updates once
  `max_function_calls` has forced `tool_choice="none"`.
- Strip `function_call` content from limit fallback responses before deciding
  whether fallback text is needed.
- Preserve meaningful metadata-only stream updates after function-call content
  is removed.
- Add regression coverage for a streaming provider returning another function
  call after the function-call budget is exhausted.
- Not claimed: microsoft/agent-framework#7334 did not merge.
- Not claimed: microsoft/agent-framework#7345 is a maintainer-authored
  consolidation PR, not a Scarab Systems pull request.
- Not claimed: This record does not claim upstream merge or final release
  inclusion for the consolidated fix.

## Validation record

- Public contribution branch was based on Microsoft Agent Framework's public
  `main` branch.
- Latest public pull request head recorded here:
  `4d437a6f4e29a02a236651b327ada039f09d14ff`.
- Targeted regression checks recorded by the public pull request:
  `uv run pytest packages/core/tests/core/test_function_invocation_logic.py -q -k 'streaming_max_function_calls_drops_post_limit_function_call_update'`.
- Limit-path regression checks recorded by the public pull request:
  `uv run pytest packages/core/tests/core/test_function_invocation_logic.py -q -k 'max_function_calls or max_iterations'`.
- Focused function-invocation suite recorded by the public pull request:
  `uv run pytest packages/core/tests/core/test_function_invocation_logic.py -q`.
- Package quality check recorded by the public pull request:
  `uv run poe check -P core`.
- Public checks visible on microsoft/agent-framework#7334 before closure
  included passing team check, label workflow, and license/CLA.

## Public review status

- microsoft/agent-framework#7334 was opened against
  `microsoft/agent-framework:main`.
- The pull request was opened from the public `scarab-systems/agent-framework`
  fork.
- The pull request is related to microsoft/agent-framework#7045.
- Public status at recording: closed without merge on 2026-07-27.
- Maintainer `eavanvalkenburg` commented that the consolidated PR
  microsoft/agent-framework#7345 incorporates the approach and extends it across
  both `max_function_calls` and `max_iterations`, including native approval
  requests while preserving metadata-only updates.
- microsoft/agent-framework#7045 remained open at recording.
- microsoft/agent-framework#7345 was open and review-required at recording.

## Public links

- https://github.com/microsoft/agent-framework/issues/7045
- https://github.com/microsoft/agent-framework/pull/7334
- https://github.com/microsoft/agent-framework/pull/7334#issuecomment-5091638927
- https://github.com/microsoft/agent-framework/pull/7345

## Changed public files

- python/packages/core/agent_framework/_tools.py
- python/packages/core/tests/core/test_function_invocation_logic.py

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
