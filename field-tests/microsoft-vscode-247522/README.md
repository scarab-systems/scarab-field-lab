---
title: "Visual Studio Code #247522"
slug: microsoft-vscode-247522
repository: microsoft/vscode
issue_url: https://github.com/microsoft/vscode/issues/247522
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-11
---
# Visual Studio Code #247522

## Public case summary

- Repository: `microsoft/vscode`
- Issue: https://github.com/microsoft/vscode/issues/247522
- Pull request: https://github.com/microsoft/vscode/pull/320877
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- SDS recorded evidence around VS Code's desktop runtime argument schema and Electron startup switch forwarding path while reviewing an accessibility regression involving NVDA and JAWS mouse echo.
- The public issue history and related Electron issue point to Chromium/Electron accessibility-provider behavior, so the repair stayed in the VS Code-owned runtime configuration surface instead of changing editor rendering.

## Repair scope

- Add `enable-features` and `disable-features` to native parsed args, CLI parser options, the `argv.json` schema, and the supported Electron switch allowlist.
- Document values as comma-separated Chromium feature names and preserve current parser behavior for empty or repeated values with focused tests.
- Not claimed: This does not fix the root Electron or Chromium accessibility bug.
- Not claimed: This does not directly change editor rendering, screen reader APIs, or mouse-hit testing behavior.
- Not claimed: This does not assert a particular Chromium feature combination is correct for every user.

## Validation record

- Dependency setup: npm ci completed so project test tools were available.
- Compile check: npm run compile-check-ts-native
- Compile result: passed
- Transpile: npm run transpile-client
- Transpile result: passed
- Node test: npm run test-node -- --run src/vs/platform/environment/test/node/argv.test.ts --grep "Chromium feature switch"
- Node test result: passed; 9 passing
- Layering check: npm run valid-layers-check
- Layering result: passed
- Diff check: git diff --check origin/main...HEAD
- Diff check result: passed
- Public PR checks at recording: license/cla passed; Dependencies Check passed; Community PR Approvals in progress.

## Public links

- https://github.com/microsoft/vscode/issues/247522
- https://github.com/microsoft/vscode/pull/320877
- https://github.com/microsoft/vscode/issues/224704
- https://github.com/electron/electron/issues/42945

## Changed public files

- src/main.ts
- src/vs/platform/environment/common/argv.ts
- src/vs/platform/environment/node/argv.ts
- src/vs/platform/environment/test/node/argv.test.ts
- src/vs/workbench/electron-browser/desktop.contribution.ts

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
