---
title: "Hummingbot #7294 and #7295"
slug: hummingbot-hummingbot-7294
repository: hummingbot/hummingbot
issue_url: https://github.com/hummingbot/hummingbot/issues/7294
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-17
---
# Hummingbot #7294 and #7295

## Public case summary

- Repository: `hummingbot/hummingbot`
- Issue: https://github.com/hummingbot/hummingbot/issues/7294
- Related issue: https://github.com/hummingbot/hummingbot/issues/7295
- Pull request: https://github.com/hummingbot/hummingbot/pull/8306
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issues reported duplicate close-order behavior after a close order
  failed or appeared to fail while trading through Hummingbot strategy
  executors.
- The repair area centered on close-order lifecycle handling in
  `PositionExecutor`.
- A close order can emit `MarketOrderFailureEvent`, be cleared for retry, and
  then still receive a legitimate late fill through the existing order-tracking
  path.
- Before the repair, the executor no longer associated that late fill with the
  failed close order, so the shutdown retry loop could submit another market
  close.

## Repair scope

- Preserve failed close-order identity long enough for `PositionExecutor` to
  reconcile a later fill for the same order.
- If that failed close order is observed as filled, restore it as the close
  order and avoid placing a duplicate close.
- Leave `ClientOrderTracker` behavior unchanged.
- Add a regression test for the failure-then-fill race.
- Not claimed: This does not change connector order-tracking semantics.
- Not claimed: This does not resolve every strategy or exchange-specific
  close-order issue.
- Not claimed: hummingbot/hummingbot#8306 has not merged at recording.

## Validation record

- Contribution branch was based on Hummingbot's public `development` branch.
- Targeted regression test for the close-order failure/fill race: passed.
- Full position executor test file: passed; 35 tests.
- Client order tracker test file: passed; 35 tests.
- Public pull request status at recording: open and ready for review.

## Public review status

- hummingbot/hummingbot#8306 is open against `hummingbot:development`.
- Maintainer edits are enabled on the pull request branch.

## Public links

- https://github.com/hummingbot/hummingbot/issues/7294
- https://github.com/hummingbot/hummingbot/issues/7295
- https://github.com/hummingbot/hummingbot/pull/8306

## Changed public files

- hummingbot/strategy_v2/executors/position_executor/position_executor.py
- test/hummingbot/strategy_v2/executors/position_executor/test_position_executor.py

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
