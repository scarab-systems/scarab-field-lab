---
title: "Microsoft Agent Framework #7503"
slug: microsoft-agent-framework-7503
repository: microsoft/agent-framework
issue_url: https://github.com/microsoft/agent-framework/issues/7503
mode: diagnostic-proof-and-repair
status: upstream-closed
recorded_at: 2026-08-04
---
# Microsoft Agent Framework #7503

## Public case summary

- Repository: `microsoft/agent-framework`
- Issue: https://github.com/microsoft/agent-framework/issues/7503
- Pull request: https://github.com/microsoft/agent-framework/pull/7512
- Mode: diagnostic-proof-and-repair
- Status: upstream-closed
- Upstream status: pull request closed without merge on 2026-08-06.
- Issue status: microsoft/agent-framework#7503 was closed upstream on
  2026-08-11.

## Diagnostic finding

- The public issue reports that Foundry hosted agents did not persist and reuse
  a hosted `agent_session_id` across turns.
- The existing Agent Framework session update path can persist
  `ChatResponse.conversation_id` into `session.service_session_id`, and the
  options path can send that hosted session ID back as
  `extra_body.agent_session_id`.
- The useful repair boundary was the first-turn Foundry response parser: when
  no hosted session ID had been sent yet, the parser could leave a transient
  Responses API ID as the conversation ID instead of promoting the durable
  hosted session handle.

## Repair scope

- Add a Foundry-local helper for reading `agent_session_id` or `session.id`
  from raw Foundry response payloads.
- Promote the hosted session ID into `conversation_id` for first-turn
  non-streaming responses and streaming response events.
- Preserve `store=False` semantics by not promoting hosted session IDs when
  response storage is explicitly disabled.
- Keep existing continuation behavior that suppresses transient response IDs
  when a hosted session ID was already supplied.
- Add Foundry package regression coverage for hosted session ID precedence and
  mapping payload shapes.
- Not claimed: microsoft/agent-framework#7512 did not merge.
- Not claimed: This record does not claim final upstream design ownership for
  Foundry hosted session multiplexing.

## Validation record

- Public contribution branch was based on Microsoft Agent Framework's public
  `main` branch.
- Latest public pull request head recorded here:
  `15b16a75ddb2065d4a34412433fe399215990330`.
- Public pull request validation recorded:
  `uv run pytest packages/foundry/tests/foundry/test_foundry_agent.py -k "hosted_agent_session_id or suppresses_conversation_id_for_agent_sessions"`.
- Public pull request validation recorded:
  `uv run pytest packages/foundry/tests/foundry/test_foundry_agent.py`.
- Public pull request validation recorded: `uv run poe test -P foundry`.
- Public pull request validation recorded: `uv run poe syntax`.
- Public pull request validation recorded:
  `uv run poe --directory packages/foundry syntax`.
- Public pull request validation recorded:
  `uv run poe --directory packages/foundry pyright`.
- Public pull request validation recorded:
  `uv run poe --directory packages/foundry build`.
- Public pull request validation recorded that `uv run poe check` reached type
  checking, including `packages/foundry`, then failed in unrelated optional
  dependency imports in `packages/lab` and `packages/core` in the fresh
  container environment.
- Automated review status visible at recording included Copilot approval.

## Public review status

- microsoft/agent-framework#7512 was opened against
  `microsoft/agent-framework:main`.
- The pull request was opened from the public `scarab-systems/agent-framework`
  fork.
- The pull request was linked to microsoft/agent-framework#7503.
- The pull request was closed without merge on 2026-08-06.
- microsoft/agent-framework#7503 was closed upstream on 2026-08-11.

## Public links

- https://github.com/microsoft/agent-framework/issues/7503
- https://github.com/microsoft/agent-framework/pull/7512

## Changed public files

- python/packages/foundry/agent_framework_foundry/_agent.py
- python/packages/foundry/tests/foundry/test_foundry_agent.py

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
