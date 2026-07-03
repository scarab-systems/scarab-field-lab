---
title: "xdg-desktop-portal #1947"
slug: flatpak-xdg-desktop-portal-1947
repository: flatpak/xdg-desktop-portal
issue_url: https://github.com/flatpak/xdg-desktop-portal/issues/1947
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-03
---
# xdg-desktop-portal #1947

## Public case summary

- Repository: `flatpak/xdg-desktop-portal`
- Issue: https://github.com/flatpak/xdg-desktop-portal/issues/1947
- Pull request: https://github.com/flatpak/xdg-desktop-portal/pull/2054
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported that an autostarted Flatpak application could be
  visible in `flatpak ps` while missing from GNOME's background apps surface.
- The reported state changed after opening and closing an application window,
  which indicated that the background app list was updated only after a later
  app or instance change.
- Target source review confirmed that `desktop-portal/background.c` refreshed
  background app state in response to running-app and instance-change events,
  but did not start an initial background check when the Background portal was
  initialized.
- The actual D-Bus property is `BackgroundApps`; the issue spelling
  `BackgroudApps` was reporter text, not source authority.
- The repair boundary was the Background portal startup monitor path, not GNOME
  Shell, Valent, Flatpak packaging, or a distribution-specific change.

## Repair scope

- Start the background monitor when the Background portal is initialized.
- Mark the startup run as an initial check so it calls
  `check_background_apps()` once without the delayed two-pass behavior used for
  later event-driven checks.
- Preserve the existing delayed two-check monitor behavior for later
  running-app and instance-change events.
- Keep the public Background portal interface unchanged.
- Not claimed: flatpak/xdg-desktop-portal#2054 has not merged at recording.
- Not claimed: This record does not claim runtime verification across every
  desktop environment, Flatpak application, or distribution package.

## AI coding-agent efficiency audit

This case also records a public summary of AI coding-agent context efficiency
for the patch decision and public PR update.

- Cold baseline context: public issue snapshot plus target repository snapshot.
- Cold baseline token use: 876,259 input tokens; 4,954 output tokens; 881,213
  total tokens.
- Scarab-guided initial repair context: selected diagnostic evidence, inspected
  target-source snippets, and verification excerpts.
- Scarab-guided initial repair context size: 3,623 tokens.
- Broader recorded original PR bundle: 4,465 tokens.
- Additional PR update and CI-fix context: 4,690 tokens.
- Combined recorded PR/update bundle: 9,155 tokens.
- Result: both approaches found the same main causal shape: the Background
  portal needed a startup background check for apps already running before
  portal startup.
- The Scarab-guided initial repair context was about 241.9x smaller than the
  cold baseline input context, a reduction of about 99.6%.
- Including the later public PR update and CI-fix work, the combined recorded
  guided bundle was still about 95.7x smaller than the cold baseline input
  context, a reduction of about 99.0%.

These token totals are estimates. This comparison describes recorded context
used for the patch decision and public PR update. It is not a claim about total
project effort, total system runtime, or maintainer review cost.

## Validation record

- Public pull request records one changed file:
  `desktop-portal/background.c`.
- Public pull request diff records 19 insertions and 5 deletions.
- Contributor validation recorded in the pull request: `git diff --check`.
- Contributor validation recorded in the pull request:
  `meson setup _build -Dtests=enabled -Dinstalled-tests=true -Dwerror=true`.
- Contributor validation recorded in the pull request: `ninja -C _build`.
- Contributor validation recorded in the pull request:
  `meson test -C _build integration/background --print-errorlogs`.
- Contributor validation recorded in the pull request:
  `meson test -C _build --suite unit --print-errorlogs`.
- Follow-up validation recorded after the first public patch revision:
  targeted notification testing completed without the previous timeout, and a
  narrower notification selection passed 42/42.
- Public checks visible at recording: build container, check container, gcc
  build-and-test, clang build-and-test, checks, and documentation build
  succeeded; documentation deploy was skipped.

## Public review status

- flatpak/xdg-desktop-portal#2054 is open against
  `flatpak/xdg-desktop-portal:main`.
- The pull request was opened from the public
  `scarab-systems/background-monitor-startup-sync` branch.
- The pull request is related to flatpak/xdg-desktop-portal#1947.
- Public status at recording: open, ready for review, mergeable, all completed
  public checks visible through GitHub passed, and not merged.

## Public links

- https://github.com/flatpak/xdg-desktop-portal/issues/1947
- https://github.com/flatpak/xdg-desktop-portal/pull/2054

## Changed public files

- desktop-portal/background.c

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, status, and
summary token-efficiency totals only.
