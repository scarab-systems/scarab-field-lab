---
title: "Open WebUI #25235"
slug: open-webui-open-webui-25235
repository: open-webui/open-webui
issue_url: https://github.com/open-webui/open-webui/issues/25235
mode: repair
status: upstream-pr-recorded
recorded_at: 2026-05-31
---
# Open WebUI #25235

## Public case summary

- Repository: `open-webui/open-webui`
- Issue: https://github.com/open-webui/open-webui/issues/25235
- Mode: repair
- Status: upstream-pr-recorded

## Diagnostic finding

- None recorded in this case.

## Repair scope

- Normalize optional chat_id metadata before prefix checks in background task handling.
- A background task could return an HTTP 400 or empty response when optional chat metadata was missing or non-string.
- AttributeError: 'NoneType' object has no attribute 'startswith'
- Not claimed: Do not claim to fix every unsafe startswith location in the repository.
- Not claimed: Do not broaden the patch beyond the reproduced background_tasks_handler chat_id path.

## Validation record

- Baseline before repair: Unpatched baseline reproduced AttributeError: 'NoneType' object has no attribute 'startswith'.
- Focused pytest: uv run --with aiosqlite --python 3.12 --group dev pytest backend/open_webui/test/util/test_middleware_chat_id_boundary.py -q
- Focused pytest result: 2 passed, 7 warnings in 5.83s
- Ruff test check: uv run --with aiosqlite --python 3.12 --group dev ruff check backend/open_webui/test/util/test_middleware_chat_id_boundary.py
- Ruff test result: All checks passed!
- Ruff middleware check: uv run --with aiosqlite --python 3.12 --group dev ruff check --select F821,F822,F823 backend/open_webui/utils/middleware.py
- Ruff middleware result: All checks passed!
- Ruff scope note: Full-file ruff on middleware.py was not claimed because unrelated upstream lint debt remains outside this patch.

## Public links

- https://github.com/open-webui/open-webui/issues/25235
- https://github.com/open-webui/open-webui/pull/25276

## Changed public files

- backend/open_webui/test/util/test_middleware_chat_id_boundary.py
- backend/open_webui/utils/middleware.py

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
