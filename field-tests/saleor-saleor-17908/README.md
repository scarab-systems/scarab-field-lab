---
title: "Saleor #17908"
slug: saleor-saleor-17908
repository: saleor/saleor
issue_url: https://github.com/saleor/saleor/issues/17908
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-12
---
# Saleor #17908

## Public case summary

- Repository: `saleor/saleor`
- Issue: https://github.com/saleor/saleor/issues/17908
- Pull request: https://github.com/saleor/saleor/pull/19453
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reports an unhandled PostgreSQL unique-constraint failure
  from `productBulkCreate` when duplicate product slugs reach `bulk_create`.
- The issue is publicly labeled `bug`, `bulk`, and `help wanted`.
- The repair area centered on product bulk-create input validation before the
  database write.
- The bulk-create path already validates some duplicate batch values before
  save, but explicit product slugs could still collide with an existing product
  slug or with another row in the same request.
- That allowed the mutation to bypass row-level validation and fail later at
  the `product_product_slug_key` database constraint instead of returning
  structured mutation errors.

## Repair scope

- Collect explicit slugs from the submitted batch before row validation.
- Detect duplicate explicit slugs in the request before `bulk_create`.
- Check submitted explicit slugs that already exist with a single database
  query.
- Return structured `UNIQUE` errors for affected product rows.
- Reserve valid explicit slugs so generated slugs later in the same batch avoid
  colliding with them.
- Add regression tests for existing slug collisions, duplicated input slugs,
  and generated slug collisions with explicit batch slugs.
- Not claimed: This does not redesign Saleor's product bulk-create API.
- Not claimed: This does not change migration or schema behavior.
- Not claimed: saleor/saleor#19453 has not merged at recording.

## Validation record

- Public contribution branch was based on Saleor's public `main` branch.
- Latest public pull request head recorded here:
  `2a27fea9c30efc13a0c1d0375afaf9b5966924c4`.
- Focused regression tests passed:
  `uv run poe test saleor/graphql/product/tests/mutations/test_product_bulk_create.py::test_product_bulk_create_with_existing_slug_and_reject_failed_rows saleor/graphql/product/tests/mutations/test_product_bulk_create.py::test_product_bulk_create_with_duplicated_slug_in_input saleor/graphql/product/tests/mutations/test_product_bulk_create.py::test_product_bulk_create_generates_slug_unique_to_explicit_batch_slug -n0`.
- Product bulk-create mutation test file passed:
  `uv run poe test saleor/graphql/product/tests/mutations/test_product_bulk_create.py -n0`.
- Touched-file lint passed:
  `uv run ruff check saleor/graphql/product/bulk_mutations/product_bulk_create.py saleor/graphql/product/tests/mutations/test_product_bulk_create.py`.
- Diff whitespace check passed: `git diff --check`.
- Public pull request status at recording: open and not draft, with upstream
  review required.
- Public checks visible at recording: one Socket check had passed and one Socket
  check was still in progress.
- Not claimed: This record does not claim upstream review, CI completion, or
  merge.

## Public review status

- saleor/saleor#19453 is open against `saleor/saleor:main`.
- The pull request was opened from the public `scarab-systems/saleor` fork.
- The pull request is linked as fixing saleor/saleor#17908.
- GitHub's review-decision field showed review required at recording.
- GitHub's merge-state field showed blocked while review and checks were still
  pending.

## Public links

- https://github.com/saleor/saleor/issues/17908
- https://github.com/saleor/saleor/pull/19453

## Changed public files

- saleor/graphql/product/bulk_mutations/product_bulk_create.py
- saleor/graphql/product/tests/mutations/test_product_bulk_create.py

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
