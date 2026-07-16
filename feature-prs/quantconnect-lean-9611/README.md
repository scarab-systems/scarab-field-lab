---
title: "QuantConnect Lean #9611"
slug: quantconnect-lean-9611
repository: QuantConnect/Lean
issue_url: https://github.com/QuantConnect/Lean/issues/7031
pull_request_url: https://github.com/QuantConnect/Lean/pull/9611
mode: feature-pr
status: upstream-pr-recorded
opened_at: 2026-07-13
recorded_at: 2026-07-16
---
# QuantConnect Lean #9611

## Public feature summary

- Repository: `QuantConnect/Lean`
- Related issue: https://github.com/QuantConnect/Lean/issues/7031
- Pull request: https://github.com/QuantConnect/Lean/pull/9611
- Mode: feature-pr
- Status: upstream-pr-recorded
- Public pull request title: "Add scheduled walk-forward optimization support"
- Related issue labels at recording: `feature`, `impact-high`, `up-next`

## Feature request context

- The public issue requests a walk-forward optimization API in LEAN.
- The requested behavior includes scheduled optimizations through
  `QCAlgorithm.Optimize(...)`, using date and time rules.
- The issue asks for existing parameter and optimization-controller technology
  to support scheduled optimization.
- The issue also asks for a pluggable optimization provider, with local
  execution for backtesting and cloud-backed execution for live trading.

## Feature scope

- Add `QCAlgorithm.Optimize(...)` overloads for scheduled walk-forward
  optimization.
- Add a provider abstraction for walk-forward optimization execution.
- Add local optimizer and QuantConnect API-backed provider paths.
- Add result plumbing for selected parameter sets and backtest summaries.
- Add safeguards against nested child optimizations.
- Add configuration keys for local and cloud-backed walk-forward optimization
  routing.
- Not claimed: This record does not claim upstream acceptance or merge.
- Not claimed: This record does not claim the full repository test suite was
  run locally.
- Not claimed: This record does not claim documentation updates have landed.

## Validation record

- Public contribution branch was based on QuantConnect Lean's public `master`
  branch.
- Public PR validation summary records:
  `dotnet build Tests/QuantConnect.Tests.csproj /p:Configuration=Debug /v:quiet`
- Public PR validation summary records 13 focused walk-forward optimization
  tests passing.
- Public PR validation summary records 186 optimizer regression tests passing.
- Public PR validation summary records:
  `git diff --check origin/master...HEAD`
- The public PR checklist explicitly leaves full-suite completion unchecked and
  states that the full repository suite was not run locally.

## Public review status

- QuantConnect/Lean#9611 is open against `QuantConnect/Lean:master`.
- The pull request was opened from the public `scarab-systems/Lean` fork.
- The public branch name is `feature-7031-walk-forward-optimization`.
- Latest public pull request head recorded here:
  `372b67d79a5bcf117632304e7344804df80d349e`
- Public pull request status at recording: open and not draft.
- Maintainer edits were enabled at recording.
- GitHub review status at recording: review required.
- GitHub merge-state field at recording: blocked.

## Public links

- https://github.com/QuantConnect/Lean/issues/7031
- https://github.com/QuantConnect/Lean/pull/9611

## Changed public areas

- Scheduled optimization API surface in `QCAlgorithm`.
- Python wrapper pass-through for the public algorithm API.
- Walk-forward optimization provider interfaces and request/result models.
- Local and API-backed walk-forward optimization providers.
- Engine routing through the local Lean manager.
- Optimizer result plumbing for selected backtest summaries.
- Launcher configuration defaults for walk-forward optimization provider
  routing.
- Focused tests for algorithm API behavior, provider routing, API provider
  selection, local provider behavior, and optimizer regressions.

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This feature record publishes public links, feature scope, validation summaries,
and status only.
