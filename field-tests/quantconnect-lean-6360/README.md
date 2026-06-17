---
title: "QuantConnect Lean #6360"
slug: quantconnect-lean-6360
repository: QuantConnect/Lean
issue_url: https://github.com/QuantConnect/Lean/issues/6360
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-17
---
# QuantConnect Lean #6360

## Public case summary

- Repository: `QuantConnect/Lean`
- Issue: https://github.com/QuantConnect/Lean/issues/6360
- Pull request: https://github.com/QuantConnect/Lean/pull/9539
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported that `PortfolioTarget.Percent` could size an option
  target above the requested portfolio weight when quote data had a bid/ask
  spread.
- The repair area centered on option target sizing through the option buying
  power model's initial margin calculation.
- The option margin path used the option mark or last price when computing the
  premium and margin value for target quantities, even when executable bid/ask
  quote prices were available.

## Repair scope

- Use the ask price for positive option target quantities when an ask is
  available.
- Use the bid price for negative option target quantities when a bid is
  available.
- Preserve the existing mark/last price fallback when quote prices are not
  available.
- Add focused regression tests for ask-priced long option margin and bid-priced
  short option margin.
- Not claimed: This does not redesign generic buying power behavior for all
  security types.
- Not claimed: QuantConnect/Lean#9539 has not merged at recording.

## Validation record

- Public contribution branch was based on QuantConnect Lean's public `master`
  branch.
- Local Release build in QuantConnect's foundation container passed with zero
  compile errors.
- Focused option-margin test class passed in the same container: 41 passed, 0
  failed.
- A local full-suite run in the available Linux arm64 foundation container
  completed with 35,966 passed, 26 skipped, and 2 failures.
- The two local full-suite failures reproduced on unchanged upstream `master`
  in the same arm64 container environment.
- Public pull request status at recording: open and ready for review.
- Not claimed: This record does not claim all upstream CI checks have completed.

## Public review status

- QuantConnect/Lean#9539 is open against `QuantConnect:master`.
- The pull request was opened from the public `scarab-systems/Lean` fork.

## Public links

- https://github.com/QuantConnect/Lean/issues/6360
- https://github.com/QuantConnect/Lean/pull/9539

## Changed public files

- Common/Securities/Option/OptionMarginModel.cs
- Tests/Common/Securities/OptionMarginBuyingPowerModelTests.cs

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
