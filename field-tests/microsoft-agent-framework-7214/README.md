---
title: "Microsoft Agent Framework #7214"
slug: microsoft-agent-framework-7214
repository: microsoft/agent-framework
issue_url: https://github.com/microsoft/agent-framework/issues/7214
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-07-30
---
# Microsoft Agent Framework #7214

## Public case summary

- Repository: `microsoft/agent-framework`
- Issue: https://github.com/microsoft/agent-framework/issues/7214
- Pull request: https://github.com/microsoft/agent-framework/pull/7375
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: microsoft/agent-framework#7375 merged on 2026-07-30.

## Diagnostic finding

- The public issue reported that Python `SummarizationStrategy` could send the
  full selected transcript to the summarizer in one provider call.
- Large histories could exceed the provider's context limit, causing
  summarization to stop contributing for that round while only logging warning
  signals.
- The useful repair boundary was the Python core compaction strategy's
  summarizer input selection and failure signaling, not provider-specific chat
  clients or session storage.
- The patch needed to keep complete message groups intact so function-call and
  function-result groupings were not split while enforcing a summarizer input
  budget.

## Repair scope

- Add a configurable `max_summary_input_tokens` budget for
  `SummarizationStrategy`.
- Use the existing character-estimator tokenizer as the default input-size
  estimator.
- Select complete message groups until the next group would exceed the
  summarizer input budget.
- Skip individually over-budget leading groups so later compactable groups can
  still be summarized.
- Mark only the groups sent to the summarizer as summarized and excluded,
  leaving unsent groups eligible for later compaction.
- Escalate repeated summarizer failures after three consecutive failures and
  reset that escalation after a successful summary.
- Add regression coverage for bounded input, oversized leading groups, repeated
  failure escalation, and escalation reset after recovery.

## Validation record

- Public pull request head before merge:
  `fc0e00c1f4be2e0d9e23f1ec194142afcfcbb46e`.
- Public pull request checks before merge included passing merge gatekeeper,
  pre-commit hooks, Python package checks, samples and Markdown, test typing
  checks, Python coverage, Python tests across supported Ubuntu and Windows
  versions, Python integration aggregate gate, dotnet aggregate path gate, and
  license/CLA.
- Python coverage automation reported 90% coverage with 9,527 tests, 34
  skipped, 0 failures, and 0 errors.
- Some integration and dotnet shard checks were skipped by the repository's
  path filtering for this Python core change.
- Public pull request status update: merged on 2026-07-30.
- Merge commit: microsoft/agent-framework@32928e645b9c2a1e527594e1d979c965d836b2c5

## Public review status

- microsoft/agent-framework#7375 was opened against
  `microsoft/agent-framework:main`.
- The pull request was opened from the public
  `scarab-systems/agent-framework` fork.
- The pull request fixed microsoft/agent-framework#7214.
- The pull request received approval from `moonbox3`.
- Microsoft Agent Framework maintainer `eavanvalkenburg` merged the pull
  request into `microsoft/agent-framework:main` on 2026-07-30.
- The issue was closed as completed on 2026-07-30 after the pull request
  merged.

## Public links

- https://github.com/microsoft/agent-framework/issues/7214
- https://github.com/microsoft/agent-framework/pull/7375
- https://github.com/microsoft/agent-framework/pull/7375#event-28713617788
- https://github.com/microsoft/agent-framework/commit/32928e645b9c2a1e527594e1d979c965d836b2c5

## Changed public files

- python/packages/core/agent_framework/_compaction.py
- python/packages/core/tests/core/test_compaction.py

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
