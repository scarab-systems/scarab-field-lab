---
title: "xdg-desktop-portal #1947"
slug: flatpak-xdg-desktop-portal-1947
repository: flatpak/xdg-desktop-portal
issue_url: https://github.com/flatpak/xdg-desktop-portal/issues/1947
mode: diagnostic-proof-and-repair
status: upstream-closed
recorded_at: 2026-07-03
---
# xdg-desktop-portal #1947

## Public case summary

- Repository: `flatpak/xdg-desktop-portal`
- Issue: https://github.com/flatpak/xdg-desktop-portal/issues/1947
- Pull request: https://github.com/flatpak/xdg-desktop-portal/pull/2054
- Mode: diagnostic-proof-and-repair
- Status: upstream-closed
- Review outcome: closed without merge

## Diagnostic finding

- The public issue reported that an autostarted Flatpak application could be
  visible in `flatpak ps` while missing from GNOME's background apps surface.
- The reported state changed after opening and closing an application window,
  which indicated that the background app list was updated only after a later
  app or instance change.
- Target source review confirmed that `desktop-portal/background.c` refreshed
  background app state when running-app and instance-change work fired, but
  existing instances needed discovery when monitoring began so `BackgroundApps`
  would be populated without waiting for another app or window event.
- The actual D-Bus property is `BackgroundApps`; the issue spelling
  `BackgroudApps` was reporter text, not source authority.
- The repair boundary was the Background portal monitor-start path, not GNOME
  Shell, Valent, Flatpak packaging, or a distribution-specific change.

## Repair scope

- Discover current background apps at the start of the background monitor cycle.
- Start the monitor cycle after the backend `RunningApplicationsChanged` signal
  and Flatpak instance-directory monitor are connected.
- Preserve the existing delayed two-check monitor behavior for later
  running-app and instance-change events.
- Add regression coverage for an app that already has a Flatpak instance before
  background monitoring starts, and verify that `BackgroundApps` lists it.
- Keep the public Background portal interface unchanged.
- Not claimed: flatpak/xdg-desktop-portal#2054 was closed without merge.
- Not claimed: This record does not claim runtime verification across every
  desktop environment, Flatpak application, or distribution package.

## Context efficiency audit

This case also records a public summary of context efficiency for the patch
decision and public PR update.

- Cold baseline context: public issue snapshot plus target repository snapshot.
- Cold baseline token use: 876,259 input tokens; 4,954 output tokens; 881,213
  total tokens.
- Scarab-guided initial repair context: selected diagnostic evidence, inspected
  target-source snippets, and verification excerpts.
- Scarab-guided initial repair context size: 3,623 tokens.
- Broader recorded original PR bundle: 4,465 tokens.
- Additional PR update and CI-fix context: 4,690 tokens.
- Combined recorded PR/update bundle: 9,155 tokens.
- Result: both approaches found the same main causal shape: `BackgroundApps`
  could miss apps already running before background monitoring discovered
  current state.
- The Scarab-guided initial repair context was about 241.9x smaller than the
  cold baseline input context, a reduction of about 99.6%.
- Including the later public PR update and CI-fix work, the combined recorded
  guided bundle was still about 95.7x smaller than the cold baseline input
  context, a reduction of about 99.0%.

These token totals are estimates. This comparison describes recorded context
used for the patch decision and public PR update. It is not a claim about total
project effort, total system runtime, or maintainer review cost. The
2026-07-06 maintainer-review revision changed the patch shape from an
initialization-time check to monitor-start discovery; these token totals were
not recalculated for that later review cycle.

## Validation record

- Public pull request currently records three changed files:
  `desktop-portal/background.c`, `tests/templates/background.py`, and
  `tests/test_background.py`.
- Public pull request diff currently records 89 insertions and 3 deletions.
- Contributor validation recorded for the current patch revision:
  `git diff --check`.
- Contributor validation recorded for the 2026-07-06 monitor-start revision:
  Python syntax compilation for `tests/test_background.py` and
  `tests/templates/background.py`.
- Local integration validation for the 2026-07-06 monitor-start revision was
  not completed because the contributor environment did not have pytest and a
  configured Meson build directory available at that point.
- The original public PR revision also recorded:
  `meson setup _build -Dtests=enabled -Dinstalled-tests=true -Dwerror=true`.
- The original public PR revision also recorded: `ninja -C _build`.
- The original public PR revision also recorded:
  `meson test -C _build integration/background --print-errorlogs`.
- The original public PR revision also recorded:
  `meson test -C _build --suite unit --print-errorlogs`.
- Follow-up validation recorded after the first public patch revision:
  targeted notification testing completed without the previous timeout, and a
  narrower notification selection passed 42/42.
- Public checks visible at latest update: build container, check container, and
  documentation build passed; documentation deploy was skipped; the Check job
  failed; gcc and clang build-and-test jobs were still pending.

## Public review status

- flatpak/xdg-desktop-portal#2054 was closed without merge on 2026-07-06.
- The pull request was opened from the public
  `scarab-systems/background-monitor-startup-sync` branch.
- The pull request is related to flatpak/xdg-desktop-portal#1947.
- Maintainer review on 2026-07-06 asked that starting monitoring discover the
  current apps instead of starting monitoring from Background portal
  initialization.
- A follow-up commit changed the repair to monitor-start discovery and added
  regression coverage.
- Public status at latest update: closed without merge on 2026-07-06.

## Public links

- https://github.com/flatpak/xdg-desktop-portal/issues/1947
- https://github.com/flatpak/xdg-desktop-portal/pull/2054
- https://github.com/flatpak/xdg-desktop-portal/pull/2054#issuecomment-4892271763
- https://github.com/flatpak/xdg-desktop-portal/pull/2054#issuecomment-4895843449
- https://github.com/flatpak/xdg-desktop-portal/pull/2054/commits/ef64757794133399990175449e18fe339622d873

## Changed public files

- desktop-portal/background.c
- tests/templates/background.py
- tests/test_background.py

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, status, and
summary token-efficiency totals only.
