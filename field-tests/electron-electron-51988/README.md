---
title: "Electron #51988"
slug: electron-electron-51988
repository: electron/electron
issue_url: https://github.com/electron/electron/issues/51988
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-07-03
---
# Electron #51988

## Public case summary

- Repository: `electron/electron`
- Issue: https://github.com/electron/electron/issues/51988
- Pull request: https://github.com/electron/electron/pull/52238
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: electron/electron#52238 merged on 2026-07-21.
- Downstream status: Arch Linux had previously carried the patch in its
  `electron42` package.

## Diagnostic finding

- The public issue reported that Electron message boxes could segfault on Linux
  when Electron was built with Qt support and Chromium selected the Qt backend,
  as seen in Arch Linux and Arch-derived desktop environments.
- The issue report identified the crash path in Electron's GTK message-box
  code: `GetGtkUiPlatform` cast the active `LinuxUi` singleton to `gtk::GtkUi`
  before asking for `GtkUiPlatform`.
- That cast was unsafe when the active Linux UI implementation was Qt rather
  than GTK. The resulting platform pointer could be invalid before Electron
  called GTK transient-window setup.
- The repair boundary was Electron's GTK message-box platform lookup, not a
  Chromium, KDE, Arch packaging, or downstream editor change.

## Repair scope

- Request the GTK-specific Linux UI theme before retrieving the GTK platform.
- Check both the GTK UI object and GTK platform object before returning the
  platform.
- Keep the existing GTK message-box implementation and public Electron API
  shape unchanged.
- Not claimed: This record does not claim upstream Electron release binaries
  were affected; the public issue notes the reported condition depends on Qt
  backend build settings.

## Validation record

- Pull request validation records:
  `yarn lint:cpp --only -- shell/browser/ui/message_box_gtk.cc`.
- Pull request validation records:
  `third_party/ninja/ninja -C out/LinuxTesting obj/electron/electron_lib/message_box_gtk.o`.
- Public pull request status update: merged on 2026-07-21.
- Merge commit: electron/electron@2cd3a56f2d5ef057b16b2b22033ab3aa918c65c6
- Public checks visible before merge included passing PR template, signed
  commits, release notes, semver label enforcement, faraday/cage, Linux,
  macOS, Windows, and backport-label checks.

## Public review status

- The pull request was opened from the public
  `scarab-systems/electron-51988-gtk-messagebox-platform-v2` branch.
- The pull request is related to electron/electron#51988.
- Electron maintainers approved and merged electron/electron#52238 into
  `electron/electron:main` on 2026-07-21.
- Merge commit: electron/electron@2cd3a56f2d5ef057b16b2b22033ab3aa918c65c6
- electron/electron#51988 was closed on 2026-07-21 after the pull request
  merged.
- Earlier pull request electron/electron#52234 was closed on 2026-07-02 and
  superseded by electron/electron#52238.
- Downstream carry: Arch Linux applied this patch in version 42.6.0-1 of its
  `electron42` package, and a public commenter confirmed that downstream package
  patch fixes electron/electron#51988.
- Not claimed: This record does not claim the fix has reached an upstream
  Electron release build.

## Public links

- https://github.com/electron/electron/issues/51988
- https://github.com/electron/electron/pull/52238
- https://github.com/electron/electron/pull/52238#event-28243443185
- https://github.com/electron/electron/commit/2cd3a56f2d5ef057b16b2b22033ab3aa918c65c6
- https://github.com/electron/electron/pull/52234
- https://github.com/electron/electron/pull/52238#issuecomment-4907611340
- https://gitlab.archlinux.org/archlinux/packaging/packages/electron42/-/merge_requests/1
- https://gitlab.archlinux.org/archlinux/packaging/packages/code/-/work_items/58

## Changed public files

- shell/browser/ui/message_box_gtk.cc

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
