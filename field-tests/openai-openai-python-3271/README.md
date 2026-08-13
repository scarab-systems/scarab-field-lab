---
title: "OpenAI Python #3271"
slug: openai-openai-python-3271
repository: openai/openai-python
issue_url: https://github.com/openai/openai-python/issues/3271
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-28
---
# OpenAI Python #3271

## Public case summary

- Repository: `openai/openai-python`
- Issue: https://github.com/openai/openai-python/issues/3271
- Pull request: https://github.com/openai/openai-python/pull/3545
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded
- Upstream status: open pull request recorded on 2026-07-28.

## Diagnostic finding

- The public issue requests Azure OpenAI and Foundry parity for promoting the
  `x-ms-served-model` response header into Responses API `Response.model`.
- The useful repair boundary was Azure client response processing in
  hand-written SDK code, not generated resource files.
- Non-empty Azure served-model headers should update normal Responses API
  response objects and streamed response events that carry
  `event.response.model`.

## Repair scope

- Promote non-empty Azure `x-ms-served-model` response headers into Responses
  API `Response.model`.
- Apply the same value to streamed response events that include
  `event.response.model`.
- Keep the change scoped to Azure client response processing.
- Add Azure Responses tests for normal and streamed paths.
- Preserve generated resource files.
- Not claimed: This does not change the OpenAI API or Azure API response
  contracts.
- Not claimed: This does not alter non-Azure client response processing.

## Validation record

- Public contribution branch was based on OpenAI Python's public `main` branch.
- Latest public pull request head recorded here:
  `51de468cc7f272f684d17f7e1d171d7bb52681a6`.
- Public pull request validation recorded:
  `.venv/bin/pytest tests/lib/test_azure.py -q -o addopts=""`.
- Public pull request validation recorded:
  `.venv/bin/ruff check src/openai/lib/azure.py tests/lib/test_azure.py`.
- Public pull request validation recorded:
  `.venv/bin/ruff format --check src/openai/lib/azure.py tests/lib/test_azure.py`.
- Public pull request validation recorded:
  `.venv/bin/pyright --pythonpath .venv/bin/python src/openai/lib/azure.py tests/lib/test_azure.py`.
- Public pull request validation recorded:
  `.venv/bin/mypy src/openai/lib/azure.py`.

## Public review status

- openai/openai-python#3545 is open against `openai/openai-python:main`.
- The pull request was opened from the public `scarab-systems/openai-python`
  fork.
- The pull request fixes openai/openai-python#3271.
- Maintainer edits are enabled.
- openai/openai-python#3271 remains open at recording.

## Public links

- https://github.com/openai/openai-python/issues/3271
- https://github.com/openai/openai-python/pull/3545

## Changed public files

- src/openai/lib/azure.py
- tests/lib/test_azure_responses.py

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
