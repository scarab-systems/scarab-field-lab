---
title: "xdg-desktop-portal-wlr #379"
slug: emersion-xdg-desktop-portal-wlr-379
repository: emersion/xdg-desktop-portal-wlr
issue_url: https://github.com/emersion/xdg-desktop-portal-wlr/issues/379
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-03
---
# xdg-desktop-portal-wlr #379

## Public case summary

- Repository: `emersion/xdg-desktop-portal-wlr`
- Issue: https://github.com/emersion/xdg-desktop-portal-wlr/issues/379
- Pull request: https://github.com/emersion/xdg-desktop-portal-wlr/pull/393
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported that `SelectSources` could fail with
  `wlroots: No supported targets specified` when the caller omitted the
  optional `types` entry.
- The issue pointed to the ScreenCast portal default: omitted `types` should
  behave as monitor capture.
- The target source path initialized `type_mask` to `0` before reading
  options, then passed that value into target setup when no explicit `types`
  value was present.
- The repair boundary was the `SelectSources` option default in
  `src/screencast/screencast.c`, not PipeWire, wlroots capture setup, a portal
  client, or distribution packaging.

## Repair scope

- Initialize `type_mask` to `MONITOR` before parsing the `SelectSources`
  options dictionary.
- Preserve existing behavior for callers that explicitly provide `types`;
  explicit option parsing still overwrites the default.
- Keep the public portal method shape unchanged.
- Not claimed: emersion/xdg-desktop-portal-wlr#393 has not merged at
  recording.
- Not claimed: This record does not claim runtime verification across every
  Wayland compositor, PipeWire setup, or distribution package.

## AI coding-agent efficiency audit

This case also records a public summary of AI coding-agent context efficiency
for the final patch decision.

- Cold baseline context: public issue snapshot plus target repository snapshot.
- Cold baseline token use: 70,212 input tokens; 1,982 output tokens; 72,194
  total tokens.
- Scarab-guided patch-decision context: selected diagnostic evidence, inspected
  target-source snippets, and verification excerpts.
- Scarab-guided final context: 2,651 tokens.
- Broader recorded comparison bundle: 6,445 tokens.
- Result: both approaches converged on the same public one-line source change,
  but the Scarab-guided final patch-decision context was about 26.5x smaller
  than the cold baseline input context, a reduction of about 96.2%.
- Using the broader recorded comparison bundle, the guided record was still
  about 10.9x smaller than the cold baseline input context, a reduction of
  about 90.8%.

This comparison describes the context used for the final patch decision. It is
not a claim about total project effort, total system runtime, or maintainer
review cost.

## Validation record

- Public pull request records one changed file:
  `src/screencast/screencast.c`.
- Public pull request diff records one insertion and one deletion.
- Contributor validation recorded in the pull request: `git diff --check`.
- Contributor validation recorded in the pull request: Fedora 44 Meson/Ninja
  build completed and linked `xdg-desktop-portal-wlr`.
- Public checks visible at recording: builds.sr.ht Alpine, Arch Linux, and
  FreeBSD checks succeeded.

## Public review status

- emersion/xdg-desktop-portal-wlr#393 is open against
  `emersion/xdg-desktop-portal-wlr:master`.
- The pull request was opened from the public
  `scarab-systems/screencast-selectsources-default-monitor` branch.
- The pull request is related to emersion/xdg-desktop-portal-wlr#379.
- Public status at recording: open, ready for review, mergeable, and not
  merged.

## Public links

- https://github.com/emersion/xdg-desktop-portal-wlr/issues/379
- https://github.com/emersion/xdg-desktop-portal-wlr/pull/393

## Changed public files

- src/screencast/screencast.c

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, status, and
sanitized token-efficiency totals only.
