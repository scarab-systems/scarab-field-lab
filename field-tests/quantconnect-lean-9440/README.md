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
- Public pull-request review later identified an adjusted-normalization
  boundary: shifted-roll mapping can reuse existing map-file rows, but existing
  continuous future factor files do not contain factor rows for arbitrary
  shifted roll dates.
- That review also identified a failure-placement issue: a provider-level
  exception could be logged inside subscription-worker execution instead of
  failing the unsupported configuration during subscription setup.

## Repair scope

- Add `DataMappingMode.TradingDaysBeforeExpiry`.
- Thread a tradeable-day mapping offset through continuous futures subscriptions,
  universe settings, history requests, mapping events, and the Python wrapper.
- Add an optional contract month cycle for continuous future contract-depth
  walking.
- Reuse existing `LastTradingDay` map-file rows for the new mode.
- Add tests covering map-file row reuse, contract-month-cycle walking, and
  tradeable-day offset behavior.
- Reject `TradingDaysBeforeExpiry` with adjusted continuous-future
  normalization until shifted-roll factor generation has an agreed factor
  approach.
- Move the unsupported adjusted-normalization check into subscription setup so
  the failure occurs before subscription workers are scheduled.
- Keep `Raw` shifted-roll mapping behavior unchanged.
- Not claimed: This does not add new futures map-file data.
- Not claimed: This does not redesign all continuous futures mapping modes.
- Not claimed: This does not implement shifted-roll factor generation.
- Not claimed: QuantConnect/Lean#9538 has not merged at recording.

## Validation record

- Public contribution branch was based on QuantConnect Lean's public `master`
  branch.
- Local build check passed with zero compile errors:
  `dotnet build Tests/QuantConnect.Tests.csproj --no-restore -clp:ErrorsOnly --verbosity quiet`
- Focused behavior checks for map-file reuse and contract-month-cycle walking
  passed in local verification.
- Follow-up foundation-container validation recorded in the pull request:
  `TradingDaysBeforeExpiryAdjustedNormalizationFailsBeforeCreatingSubscription`
  passed 3 cases.
- Follow-up foundation-container validation recorded in the pull request:
  `RejectsTradingDaysBeforeExpiryForAdjustedFutureFactorFiles` passed 3 cases.
- Follow-up foundation-container validation recorded in the pull request:
  `dotnet build Tests/QuantConnect.Tests.csproj` succeeded.
- Public reviewer validation recorded in the pull request: shifted `Raw`
  mapping with month cycle and contract depth was exercised over July 2013 to
  February 2016 using repository sample data.
- Public pull request status at recording: open and ready for review.
- Not claimed: This record does not claim all upstream CI checks have completed.

## Public review status

- QuantConnect/Lean#9538 is open against `QuantConnect:master`.
- The pull request was opened from the public `scarab-systems/Lean` fork.
- Public review recorded a normalization gap and recommended fail-fast handling
  for unsupported adjusted normalization.
- Follow-up commits recorded a provider-level guard and then a subscription
  setup guard for unsupported adjusted normalization.
- Latest public branch head at recording:
  `eea3e0fb8c2ff920a05cb370e9491f52ebac9385`.
- Latest public contributor reply at recording:
  https://github.com/QuantConnect/Lean/pull/9538#issuecomment-4887950631
- Public status at recording: open, not merged, with shifted-roll factor
  generation left for QuantConnect guidance or follow-up implementation.

## Public links

- https://github.com/QuantConnect/Lean/issues/9440
- https://github.com/QuantConnect/Lean/pull/9538
- https://github.com/QuantConnect/Lean/pull/9538/commits/eea3e0fb8c2ff920a05cb370e9491f52ebac9385
- https://github.com/QuantConnect/Lean/pull/9538#issuecomment-4880435658
- https://github.com/QuantConnect/Lean/pull/9538#issuecomment-4884934211
- https://github.com/QuantConnect/Lean/pull/9538#issuecomment-4887950631

## Changed public areas

- Continuous futures mapping mode selection.
- Continuous futures subscription, history, universe, and mapping-event
  configuration.
- Contract-month-cycle walking for continuous futures.
- Python wrapper pass-through for the public algorithm API.
- Focused unit-test coverage for the new mapping behavior.
- Factor-provider guard for unsupported shifted-roll adjusted normalization.
- Subscription setup guard for unsupported shifted-roll adjusted normalization.

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
