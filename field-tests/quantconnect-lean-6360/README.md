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
- The option margin path used the option mark/last price when computing the
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
- Local build check passed with zero compile errors:
  `dotnet build Tests/QuantConnect.Tests.csproj -p:RunAnalyzers=false`
- Focused option margin regression methods passed locally against the built test
  assembly.
- Direct local `dotnet test --no-build` execution in the available Linux
  container aborted during PythonNet test-host initialization before the selected
  tests ran.
- Public pull request status at recording: open draft.
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
