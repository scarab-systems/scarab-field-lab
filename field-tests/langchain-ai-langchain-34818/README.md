---
title: "LangChain #34818"
slug: langchain-ai-langchain-34818
repository: langchain-ai/langchain
issue_url: https://github.com/langchain-ai/langchain/issues/34818
mode: repair
status: repair-recorded
recorded_at: 2026-06-04
---
# LangChain #34818

## Public case summary

- Repository: `langchain-ai/langchain`
- Issue: https://github.com/langchain-ai/langchain/issues/34818
- Mode: repair
- Status: repair-recorded

## Diagnostic finding


## Repair scope

- Preserve free model choice on the first ToolStrategy turn when real tools are available, then continue forcing a structured-output tool after tool results exist so final structured response behavior remains intact.
- When an agent is created with tools and a ToolStrategy response_format, the first model turn is bound with tool_choice="any", preventing the model from freely emitting intermediate text before choosing a real tool.
- The agent factory adds structured-output tools to every ToolStrategy model turn and forced tool_choice="any" before any real tool result exists.
- Not claimed: Do not redesign create_agent response_format semantics.
- Not claimed: Do not add public API parameters.
- Not claimed: Do not claim to resolve ProviderStrategy/json_schema streaming semantics.
- Not claimed: Do not change provider adapter behavior.

## Validation record

- Regression before repair: uv run --group test pytest tests/unit_tests/agents/test_response_format.py -k "does_not_force_first_turn"
- Result before repair: failed before repair with recorded tool choices ['any', 'any'] instead of [None, 'any']
- Focused regression: uv run --group test pytest tests/unit_tests/agents/test_response_format.py -k "does_not_force_first_turn"
- Focused result: 1 passed
- Response-format suite: uv run --group test pytest tests/unit_tests/agents/test_response_format.py
- Response-format result: 30 passed
- Agent-streaming suite: uv run --group test pytest tests/unit_tests/agents/test_agent_streaming.py
- Agent-streaming result: 10 passed
- Ruff: uv run --group lint ruff format --check langchain/agents/factory.py tests/unit_tests/agents/test_response_format.py && uv run --group lint ruff check langchain/agents/factory.py tests/unit_tests/agents/test_response_format.py
- Ruff result: 2 files already formatted; all checks passed
- Mypy: uv run --group typing mypy langchain/agents/factory.py
- Mypy result: success, no issues found in 1 source file
- Diff check: git diff --check -- libs/langchain_v1/langchain/agents/factory.py libs/langchain_v1/tests/unit_tests/agents/test_response_format.py
- Diff check result: passed

## Public links

- https://github.com/langchain-ai/langchain/issues/34818
- https://github.com/langchain-ai/langchain/issues/34818#issuecomment-4626690263

## Changed public files

- libs/langchain_v1/langchain/agents/factory.py
- libs/langchain_v1/tests/unit_tests/agents/test_response_format.py

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
