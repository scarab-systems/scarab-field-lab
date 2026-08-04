---
title: "Microsoft Agent Framework #7393"
slug: microsoft-agent-framework-7393
repository: microsoft/agent-framework
issue_url: https://github.com/microsoft/agent-framework/issues/7393
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-08-04
---
# Microsoft Agent Framework #7393

## Public case summary

- Repository: `microsoft/agent-framework`
- Issue: https://github.com/microsoft/agent-framework/issues/7393
- Pull request: https://github.com/microsoft/agent-framework/pull/7396
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: microsoft/agent-framework#7396 merged on 2026-08-04.

## Diagnostic finding

- The public issue reported that Python `ToolResultCompactionStrategy` could
  exclude older tool-call groups but then reinsert a synthetic
  `[Tool results: ...]` message containing the complete unbounded result text
  for those groups.
- For read-heavy workloads, where tool-result text is the dominant context
  payload, that behavior could make a compaction pass report that it changed
  the transcript while reclaiming very little usable context.
- Already-excluded tool results also needed to stay excluded so a later digest
  pass could not restore payload text that an earlier compaction had removed.
- The useful repair boundary was the Python core tool-result digest builder,
  not provider-specific clients, caller logging, or the broader compaction
  ladder.

## Repair scope

- Build digest text only from messages still included in the selected group.
- Preserve full-group provenance links separately from digest text.
- Bound the synthesized summary text with an elision marker.
- Keep existing small-result digest behavior unchanged.
- Collapse digest scanning into a single pass after review feedback.
- Add regression coverage for oversized tool-result payloads and
  already-excluded tool results.
- Not claimed: No public API change was made.

## Validation record

- Public pull request head before merge:
  `ed38cec0cd399975f8bebf5cbbaffe2c759a78f1`.
- Public pull request checks before merge included passing merge gatekeeper,
  pre-commit hooks, Python package checks, samples and Markdown, test typing
  checks, Python coverage, Python tests across supported Ubuntu and Windows
  versions, Python integration aggregate gate, dotnet aggregate path gate, and
  license/CLA.
- Python coverage automation reported 90% coverage with 9,744 tests, 34
  skipped, 0 failures, and 0 errors.
- Some integration and dotnet shard checks were skipped by the repository's
  path filtering for this Python core change.
- Public pull request status update: merged on 2026-08-04.
- Merge commit:
  microsoft/agent-framework@4d3c7844d6fd01b49910fcb8ee7623f48f4d5939

## Public review status

- microsoft/agent-framework#7396 was opened against
  `microsoft/agent-framework:main`.
- The pull request was opened from the public `scarab-systems/agent-framework`
  fork.
- The pull request fixed microsoft/agent-framework#7393.
- The pull request received human approvals from `moonbox3` and `giles17`.
- `moonbox3` enabled auto-merge and merged the pull request into
  `microsoft/agent-framework:main` on 2026-08-04.
- The issue was closed as completed on 2026-08-04 after the pull request
  merged.

## Public links

- https://github.com/microsoft/agent-framework/issues/7393
- https://github.com/microsoft/agent-framework/pull/7396
- https://github.com/microsoft/agent-framework/pull/7396#pullrequestreview-4855977876
- https://github.com/microsoft/agent-framework/pull/7396#issuecomment-5130991790
- https://github.com/microsoft/agent-framework/commit/4d3c7844d6fd01b49910fcb8ee7623f48f4d5939

## Changed public files

- python/packages/core/agent_framework/_compaction.py
- python/packages/core/tests/core/test_compaction.py

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
