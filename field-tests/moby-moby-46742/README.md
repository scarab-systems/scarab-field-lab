---
title: "Moby #46742"
slug: moby-moby-46742
repository: moby/moby
issue_url: https://github.com/moby/moby/issues/46742
mode: diagnostic-proof
status: upstream-closed
recorded_at: 2026-06-04
---
# Moby #46742

## Public case summary

- Repository: `moby/moby`
- Issue: https://github.com/moby/moby/issues/46742
- Pull request: https://github.com/moby/moby/pull/52761
- Mode: diagnostic-proof
- Status: upstream-closed
- Review outcome: closed without merge

## Diagnostic finding

- The Moby harness contains docker-py pytest deselections with source-visible rationale comments, so the original SDS wording overstated the finding as lacking visible proof context.
- No engine or snapshotter behavior repair is claimed from this run. Draft PR https://github.com/moby/moby/pull/52761 proposes only a harness visibility improvement that preserves the existing docker-py deselection behavior while printing each selector and reason before pytest runs.

## Repair scope

- None recorded in this case.

## Validation record

- Public pull request status: closed without merge on 2026-06-05.
- Not claimed: This case does not claim an accepted Moby engine or harness
  change.

## Public review status

- moby/moby#52761 was closed without merge on 2026-06-05.
- The pull request proposed harness visibility only; no engine or snapshotter
  repair is claimed from this case.

## Public links

- https://github.com/moby/moby/issues/46742
- https://github.com/moby/moby/issues/46742#issuecomment-4622482548
- https://github.com/moby/moby/pull/52761

## Changed public files

- None recorded in this case.

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
