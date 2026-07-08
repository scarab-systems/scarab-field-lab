---
title: "Electron #51988"
slug: electron-electron-51988
repository: electron/electron
issue_url: https://github.com/electron/electron/issues/51988
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-03
---
# Electron #51988

## Public case summary

- Repository: `electron/electron`
- Issue: https://github.com/electron/electron/issues/51988
- Pull request: https://github.com/electron/electron/pull/52238
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

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
- Not claimed: electron/electron#52238 has not merged at recording.
- Not claimed: This record does not claim upstream Electron release binaries
  were affected; the public issue notes the reported condition depends on Qt
  backend build settings.

## Validation record

- Pull request validation records:
  `yarn lint:cpp --only -- shell/browser/ui/message_box_gtk.cc`.
- Pull request validation records:
  `third_party/ninja/ninja -C out/LinuxTesting obj/electron/electron_lib/message_box_gtk.o`.
- Public checks visible at recording: PR template, signed commits,
  Socket Security alerts, Socket project report, and release notes passed.
- Public checks still pending at recording: semver label enforcement,
  backport label handling, and faraday/cage.

## Public review status

- electron/electron#52238 is open against `electron/electron:main`.
- The pull request was opened from the public
  `scarab-systems/electron-51988-gtk-messagebox-platform-v2` branch.
- The pull request is related to electron/electron#51988.
- Public status at recording: open, ready for review, mergeable, and not
  merged.
- Earlier pull request electron/electron#52234 was closed on 2026-07-02 and
  superseded by electron/electron#52238.

## Public links

- https://github.com/electron/electron/issues/51988
- https://github.com/electron/electron/pull/52238
- https://github.com/electron/electron/pull/52234
- https://gitlab.archlinux.org/archlinux/packaging/packages/code/-/work_items/58
- https://github.com/microsoft/vscode/issues/320788

## Changed public files

- shell/browser/ui/message_box_gtk.cc

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
