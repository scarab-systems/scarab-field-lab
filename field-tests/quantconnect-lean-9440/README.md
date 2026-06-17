---
title: "QuantConnect Lean #9440"
slug: quantconnect-lean-9440
repository: QuantConnect/Lean
issue_url: https://github.com/QuantConnect/Lean/issues/9440
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-17
---
# QuantConnect Lean #9440

## Public case summary

- Repository: `QuantConnect/Lean`
- Issue: https://github.com/QuantConnect/Lean/issues/9440
- Pull request: https://github.com/QuantConnect/Lean/pull/9538
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue requested configurable continuous futures roll timing for
  `DataMappingMode`, plus a way to restrict held contracts to selected contract
  months.
- The repair area centered on continuous futures mapping, map-file lookup,
  contract-depth walking, and propagation through subscription and history
  configuration.
- Existing last-trading-day map-file rows could be reused for an earlier-roll
  mode if the lookup date is shifted by a configured number of tradeable days.
- Contract-depth walking also needed to respect a selected month cycle, so a
  skipped month would not be selected as the held continuous contract.

## Repair scope

- Add `DataMappingMode.TradingDaysBeforeExpiry`.
- Thread a tradeable-day mapping offset through continuous futures subscriptions,
  universe settings, history requests, mapping events, and the Python wrapper.
- Add an optional contract month cycle for continuous future contract-depth
  walking.
- Reuse existing `LastTradingDay` map-file rows for the new mode.
- Add tests covering map-file row reuse, contract-month-cycle walking, and
  tradeable-day offset behavior.
- Not claimed: This does not add new futures map-file data.
- Not claimed: This does not redesign all continuous futures mapping modes.
- Not claimed: QuantConnect/Lean#9538 has not merged at recording.

## Validation record

- Public contribution branch was based on QuantConnect Lean's public `master`
  branch.
- Local build check passed with zero compile errors:
  `dotnet build Tests/QuantConnect.Tests.csproj --no-restore -clp:ErrorsOnly --verbosity quiet`
- Focused behavior checks for map-file reuse and contract-month-cycle walking
  passed in local verification.
- Direct local NUnit execution in the available Linux container aborted during
  global Python/test-host initialization before the selected tests ran.
- Public pull request status at recording: open and ready for review.
- Not claimed: This record does not claim all upstream CI checks have completed.

## Public review status

- QuantConnect/Lean#9538 is open against `QuantConnect:master`.
- The pull request was opened from the public `scarab-systems/Lean` fork.

## Public links

- https://github.com/QuantConnect/Lean/issues/9440
- https://github.com/QuantConnect/Lean/pull/9538

## Changed public areas

- Continuous futures mapping mode selection.
- Continuous futures subscription, history, universe, and mapping-event
  configuration.
- Contract-month-cycle walking for continuous futures.
- Python wrapper pass-through for the public algorithm API.
- Focused unit-test coverage for the new mapping behavior.

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
