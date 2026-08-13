---
title: "OpenAI Python #3472"
slug: openai-openai-python-3472
repository: openai/openai-python
issue_url: https://github.com/openai/openai-python/issues/3472
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-08-13
---
# OpenAI Python #3472

## Public case summary

- Repository: `openai/openai-python`
- Issue: https://github.com/openai/openai-python/issues/3472
- Pull request: https://github.com/openai/openai-python/pull/3605
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded
- Upstream status: open pull request recorded on 2026-08-13.

## Diagnostic finding

- The public issue reported that live Responses API streams could emit
  `response.function_call_arguments.done` events without the required `name`
  field.
- In the Python SDK, strict stream-event validation expects that field on
  `ResponseFunctionCallArgumentsDoneEvent`, so the missing value could fail
  validation before application code received the completed arguments event.
- The useful repair boundary was the hand-written Responses streaming
  transport and validation path, not the generated event models.
- The stream already receives the function-call name earlier on
  `response.output_item.added`, so the missing value can be restored by
  correlating later argument events through `item_id`.

## Repair scope

- Add a small Responses stream-event normalizer in hand-written SDK code.
- Record function-call names from `response.output_item.added` events by
  `item_id`.
- Fill `response.function_call_arguments.done.name` from the correlated
  `item_id` before strict `ResponseStreamEvent` validation.
- Cover both sync and async `client.responses.create(..., stream=True)` paths.
- Preserve generated event model files.
- Not claimed: This does not change the OpenAI API event schema.
- Not claimed: This does not make isolated missing-name event payloads valid
  outside the SDK streaming state path.

## Validation record

- Public contribution branch was based on OpenAI Python's public `main` branch.
- Latest public pull request head recorded here:
  `55d02cdca4571334b5bba1f00e53013ee35a8e7b`.
- Fail-first coverage was checked before the repair against the strict missing
  `ResponseFunctionCallArgumentsDoneEvent.name` validation path.
- Public pull request validation recorded:
  `uv run pytest tests/lib/responses/test_responses.py tests/test_streaming.py tests/test_httpx2.py -o addopts=`; 47 passed.
- Public pull request validation recorded:
  `uv run nox -s test-pydantic-v1 -- tests/lib/responses/test_responses.py -k streamed_function_call_arguments_done_uses_output_item_name -o addopts=`; 2 passed.
- Public pull request validation recorded: `uv run ruff check .`.
- Public pull request validation recorded:
  `uv run ruff format --check src/openai/_streaming.py src/openai/lib/streaming/responses/_stream_event_normalizer.py tests/lib/responses/test_responses.py`.
- Public pull request validation recorded:
  `uv run pyright src/openai/_streaming.py src/openai/lib/streaming/responses/_stream_event_normalizer.py tests/lib/responses/test_responses.py`.
- Public pull request validation recorded:
  `uv run mypy src/openai/_streaming.py src/openai/lib/streaming/responses/_stream_event_normalizer.py`.
- Public pull request validation recorded: `uv run python -c 'import openai'`.

## Public review status

- openai/openai-python#3605 is open against `openai/openai-python:main`.
- The pull request was opened from the public `scarab-systems/openai-python`
  fork.
- The pull request is review-ready and maintainer edits are enabled.
- GitHub reported the pull request mergeable at recording.
- GitHub review status at recording: review required.

## Public links

- https://github.com/openai/openai-python/issues/3472
- https://github.com/openai/openai-python/pull/3605

## Changed public files

- src/openai/_streaming.py
- src/openai/lib/streaming/responses/_stream_event_normalizer.py
- tests/lib/responses/test_responses.py

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
