---
title: "Next.js #86784"
slug: vercel-next-js-86784
repository: vercel/next.js
issue_url: https://github.com/vercel/next.js/issues/86784
mode: diagnostic-proof-and-repair
status: repair-recorded
recorded_at: 2026-06-06
---
# Next.js #86784

## Public case summary

- Repository: `vercel/next.js`
- Issue: https://github.com/vercel/next.js/issues/86784
- Mode: diagnostic-proof-and-repair
- Status: repair-recorded

## Diagnostic finding

- SDS surfaced an external package syntax and transform-ownership boundary in the Next.js target without treating issue text as diagnostic truth.
- Any repair work should start from the public build configuration and transform ownership boundary.

## Repair scope

- React Native package source can carry Flow syntax through Turbopack when external package imports are routed into a browser build without transform authority.
- Keep the React Native Web example alias and .web extension precedence, then add a narrowly scoped Turbopack rule that applies babel-loader with @babel/preset-flow only to JavaScript files from the react-native package.
- Not claimed: No Turbopack parser behavior is changed.
- Not claimed: No broad node_modules transpilation is introduced.
- Not claimed: The patch is an example/config repair, not a full React Native ecosystem compatibility redesign.

## Validation record

- `node inline check requiring examples/with-react-native-web/next.config.js and asserting the rule matches node_modules/react-native while ignoring react-native-web` - passed
- `node --check examples/with-react-native-web/next.config.js && python3 -m json.tool examples/with-react-native-web/package.json` - passed
- `pnpm dlx prettier@3.6.2 --check examples/with-react-native-web/README.md examples/with-react-native-web/next.config.js examples/with-react-native-web/package.json` - passed
- `git diff HEAD^..HEAD --check` - passed
- Commit signature check - passed
- Not run: full monorepo bootstrap/build and GitHub PR checks.

## Public links

- https://github.com/vercel/next.js/issues/86784

## Changed public files

- examples/with-react-native-web/README.md
- examples/with-react-native-web/next.config.js
- examples/with-react-native-web/package.json

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
