---
title: "Open WebUI #25038"
slug: open-webui-open-webui-25038
repository: open-webui/open-webui
issue_url: https://github.com/open-webui/open-webui/issues/25038
mode: repair
status: upstream-closed
recorded_at: 2026-05-31
---
# Open WebUI #25038

## Public case summary

- Repository: `open-webui/open-webui`
- Issue: https://github.com/open-webui/open-webui/issues/25038
- Pull request: https://github.com/open-webui/open-webui/pull/25275
- Mode: repair
- Status: upstream-closed
- Review outcome: closed without merge

## Diagnostic finding

- The repair boundary was web-search generated-query response normalization
  before source/context assembly: non-OpenAI-style error responses needed a
  fallback to the original user query instead of being treated as choices
  payloads.

## Repair scope

- Guard the web search generated-query response shape and fall back to the original user query when the generated response is not an OpenAI-style choices payload.
- Web search could retrieve results, then source/context handling could fail with a JSONResponse response-shape error and report no sources.
- TypeError: 'JSONResponse' object is not subscriptable
- Not claimed: Do not claim to fix every possible no-sources-found path.
- Not claimed: Do not include the separate SafeWebBaseLoader or allow_redirects path without separate reproduction.

## Validation record

- Baseline before repair: Unpatched baseline reproduced TypeError: 'JSONResponse' object is not subscriptable.
- Focused pytest: uv run --frozen --python 3.12 --with python-mimeparse --group dev pytest backend/open_webui/test/util/test_middleware_source_context.py -q
- Focused pytest result: 2 passed in 1.85s
- Ruff import-order check: uv run --frozen --python 3.12 --with python-mimeparse --with ruff --group dev ruff check --select I backend/open_webui/utils/middleware.py backend/open_webui/utils/response.py backend/open_webui/test/util/test_middleware_source_context.py
- Ruff import-order result: All checks passed!
- Ruff targeted check: uv run --frozen --python 3.12 --with python-mimeparse --with ruff --group dev ruff check backend/open_webui/utils/response.py backend/open_webui/test/util/test_middleware_source_context.py
- Ruff targeted result: All checks passed!
- Ruff scope note: Full-file ruff on middleware.py still reports unrelated upstream lint debt outside this patch.
- Public pull request status: closed without merge on 2026-06-01.

## Public review status

- open-webui/open-webui#25275 was closed without merge on 2026-06-01.
- Not claimed: This case does not claim upstream acceptance or merge.

## Public links

- https://github.com/open-webui/open-webui/issues/25038
- https://github.com/open-webui/open-webui/pull/25275

## Changed public files

- backend/open_webui/test/util/test_middleware_source_context.py
- backend/open_webui/utils/middleware.py
- backend/open_webui/utils/response.py

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
