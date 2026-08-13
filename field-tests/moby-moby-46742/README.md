---
title: "Moby #46742"
slug: moby-moby-46742
repository: moby/moby
issue_url: https://github.com/moby/moby/issues/46742
mode: diagnostic-proof
status: upstream-pr-recorded
recorded_at: 2026-06-04
---
# Moby #46742

## Public case summary

- Repository: `moby/moby`
- Issue: https://github.com/moby/moby/issues/46742
- Pull request: https://github.com/moby/moby/pull/52762
- Superseded pull request: https://github.com/moby/moby/pull/52761
- Mode: diagnostic-proof
- Status: upstream-pr-recorded
- Upstream status: refreshed open pull request recorded on 2026-06-05.

## Diagnostic finding

- The Moby harness contains docker-py pytest deselections with source-visible
  rationale comments, so the original wording overstated the finding as lacking
  visible proof context.
- No engine or snapshotter behavior repair is claimed from this case.
- The public pull request proposes only a harness visibility improvement that
  preserves the existing docker-py deselection behavior while printing each
  selector and reason before pytest runs.

## Repair scope

- Keep existing docker-py deselection behavior unchanged.
- Collect docker-py deselection selectors with short reason text.
- Print the selector and reason list before pytest starts so CI logs show why
  each integration test was deselected.
- Not claimed: This does not fix docker-py snapshotter failures.
- Not claimed: This does not change engine, containerd, or snapshotter behavior.

## Validation record

- Public contribution branch was refreshed against Moby's public `master`
  branch.
- Latest public pull request head recorded here:
  `5fc9abb81c6d842e001771ce46cc3c4443a0876e`.
- Public pull request validation recorded:
  `bash -n hack/make/test-docker-py`.
- Public pull request validation recorded: `git diff --check`.
- Not claimed: This case does not claim an accepted Moby engine or harness
  change.

## Public review status

- moby/moby#52761 was closed without merge on 2026-06-05.
- moby/moby#52762 supersedes moby/moby#52761 on a refreshed branch.
- moby/moby#52762 is open against `moby/moby:master`.
- The pull request was opened from the public `scarab-systems/moby` fork.
- Maintainer edits are enabled.
- The pull request proposes harness visibility only; no engine or snapshotter
  repair is claimed from this case.

## Public links

- https://github.com/moby/moby/issues/46742
- https://github.com/moby/moby/issues/46742#issuecomment-4622482548
- https://github.com/moby/moby/pull/52761
- https://github.com/moby/moby/pull/52762

## Changed public files

- hack/make/test-docker-py

## Assistance disclosure

The refreshed public pull request includes an AI-assisted coding disclosure.
Public submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
