---
title: "Visual Studio Code #314457"
slug: microsoft-vscode-314457
repository: microsoft/vscode
issue_url: https://github.com/microsoft/vscode/issues/314457
mode: diagnostic-proof-and-repair
status: public-comment-recorded
recorded_at: 2026-06-04
---
# Visual Studio Code #314457

## Public case summary

- Repository: `microsoft/vscode`
- Issue: https://github.com/microsoft/vscode/issues/314457
- Mode: diagnostic-proof-and-repair
- Status: public-comment-recorded

## Diagnostic finding

- SDS surfaced an input-geometry parity boundary around VS Code editor edit-context behavior from public repository evidence.
- A local repair candidate was prepared after the SDS finding.

## Repair scope

- Round native EditContext selection/control/character bounds at the browser API boundary through a focused geometry helper.
- Input can visually jump at certain zoom levels.
- The native EditContext path can hand fractional selection, control, and character DOMRect bounds into the browser EditContext API while nearby editor rendering paths round visible geometry.
- Not claimed: Do not change broader editor layout.
- Not claimed: Do not change the text area edit-context path.
- Not claimed: Do not change browser EditContext lifecycle behavior.
- Not claimed: Do not add ad hoc issue-specific diagnostics outside SDS.

## Validation record

- Compile check before repair: npm run compile-check-ts-native failed because the new regression imported a missing nativeEditContextGeometry module.
- Compile check: npm run compile-check-ts-native
- Compile result: passed
- Transpile: npm run transpile-client
- Transpile result: passed
- Node test: npm run test-node -- --run src/vs/editor/test/browser/controller/nativeEditContextGeometry.test.ts
- Node test result: NativeEditContextGeometry passed; no unexpected test errors.
- Browser test: npm run test-browser -- --browser chromium --run src/vs/editor/test/browser/controller/nativeEditContextGeometry.test.ts
- Browser test result: NativeEditContextGeometry passed in Chromium.
- Layering check: npm run valid-layers-check
- Layering result: passed
- Diff check: git diff --check HEAD~1 HEAD
- Diff check result: passed

## Public links

- https://github.com/microsoft/vscode/issues/314457
- https://github.com/microsoft/vscode/issues/314457#issuecomment-4625693572

## Changed public files

- src/vs/editor/browser/controller/editContext/native/nativeEditContext.ts
- src/vs/editor/browser/controller/editContext/native/nativeEditContextGeometry.ts
- src/vs/editor/test/browser/controller/nativeEditContextGeometry.test.ts

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
