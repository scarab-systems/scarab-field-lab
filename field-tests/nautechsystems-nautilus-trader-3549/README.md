---
title: "NautilusTrader #3549"
slug: nautechsystems-nautilus-trader-3549
repository: nautechsystems/nautilus_trader
issue_url: https://github.com/nautechsystems/nautilus_trader/issues/3549
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-17
---
# NautilusTrader #3549

## Public case summary

- Repository: `nautechsystems/nautilus_trader`
- Issue: https://github.com/nautechsystems/nautilus_trader/issues/3549
- Pull request: https://github.com/nautechsystems/nautilus_trader/pull/4285
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported mixed-precision `OrderBookDelta` values losing
  precision after `ParquetDataCatalog` writes, reads, or consolidation.
- The repair area centered on Parquet catalog chunking and Rust-backed catalog
  query registration for order book delta files.
- `OrderBookDelta` precision is stored as file-level metadata, so a single file
  cannot safely represent rows that require different price or size precision.
- Directory-level query registration also cannot decode mixed-precision files
  together because Arrow rejects conflicting file metadata before the per-file
  decoder can preserve each precision.

## Repair scope

- Split `OrderBookDelta` writes into contiguous precision-compatible Parquet
  runs.
- Decode `OrderBookDelta` files individually when optimization is off, preserving
  each file's own precision metadata.
- Preserve the existing guard that rejects consolidation across conflicting
  metadata.
- Add a regression test for alternating mixed price and size precision.
- Not claimed: This does not change the storage contract for all market data
  types.
- Not claimed: This does not make consolidation merge files with conflicting
  metadata.
- Not claimed: nautechsystems/nautilus_trader#4285 has not merged at recording.

## Validation record

- Contribution branch was based on NautilusTrader's public `develop` branch.
- Repo-native debug build completed successfully.
- Full persistence catalog test file passed locally: 97 passed, 1 skipped.
- Focused mixed-precision order book delta regression passed locally.
- Existing conflicting-metadata consolidation guard test passed locally.
- Ruff check on changed public files passed locally.
- Python bytecode compilation for changed public files passed locally.
- Public pull request status at recording: open and ready for review.

## Public review status

- nautechsystems/nautilus_trader#4285 is open against
  `nautechsystems:develop`.
- The pull request was opened from the public `scarab-systems/nautilus_trader`
  fork.

## Public links

- https://github.com/nautechsystems/nautilus_trader/issues/3549
- https://github.com/nautechsystems/nautilus_trader/pull/4285

## Changed public files

- nautilus_trader/persistence/catalog/parquet.py
- tests/unit_tests/persistence/test_catalog.py

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
