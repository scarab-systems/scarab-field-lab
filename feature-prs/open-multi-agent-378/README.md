---
title: "Open Multi Agent #378"
slug: open-multi-agent-378
repository: open-multi-agent/open-multi-agent
issue_url: https://github.com/open-multi-agent/open-multi-agent/issues/204
pull_request_url: https://github.com/open-multi-agent/open-multi-agent/pull/378
mode: feature-pr
status: upstream-accepted
opened_at: 2026-07-16
accepted_at: 2026-07-16
recorded_at: 2026-07-25
---
# Open Multi Agent #378

## Public feature summary

- Repository: `open-multi-agent/open-multi-agent`
- Related issue: https://github.com/open-multi-agent/open-multi-agent/issues/204
- Pull request: https://github.com/open-multi-agent/open-multi-agent/pull/378
- Mode: feature-pr
- Status: upstream-accepted
- Public pull request title: "feat(agent): add generic process backend"
- Merge commit:
  open-multi-agent/open-multi-agent@fa1606cad2b9c1de03a24b47ac979e385dc61766

## Feature request context

- The public issue requested external agent orchestration so CLI tools and
  business-system adapters could participate as team members.
- Open Multi Agent already had ACP-backed external agent support.
- This pull request added a protocol-neutral `process` backend alongside ACP.
- The issue was closed after the public maintainer recorded the core external
  agent work as shipped through #360 and #378.

## Feature scope

- Add a generic process backend for local CLIs, scripts, and adapters.
- Keep ACP as the protocol-aware backend while adding the process backend as a
  simpler prompt-in/stdout-out path.
- Add process backend stdin, argument, and no-prompt delivery modes.
- Add cancellation and process-tree cleanup behavior for subprocess lifecycles.
- Add public package exports for `@open-multi-agent/core/process`.
- Add docs and examples for the process backend.
- Not claimed: This record does not claim ownership of all external-agent work
  in Open Multi Agent.
- Not claimed: This record does not claim business-system adapters landed in
  this pull request.

## Validation record

- Public PR validation summary records:
  `npm run lint && npm test`
- Public PR validation summary records:
  `npm run build`
- Public PR validation summary records:
  `npm run test:scaffold`
- Public PR validation summary records:
  `npx tsx packages/core/examples/integrations/external-agent-process.ts`
- Public PR validation summary records package entrypoint smoke checks for
  `dist/index.js`, `dist/acp.js`, `dist/process.js`, `dist/mcp.js`, and
  `dist/ai-sdk.js`.
- GitHub CI checks completed successfully before merge, including lint, test,
  coverage, package, and scaffold e2e jobs.

## Public review status

- Open Multi Agent maintainer Jack Chen reviewed the pull request.
- Public review first identified compatibility, subprocess lifecycle, and docs
  blockers.
- The public contribution was updated to preserve the ACP backend config type,
  add process-tree cleanup paths, add lifecycle regression coverage, and revise
  docs.
- The maintainer confirmed the original blockers were resolved and merged the
  pull request on 2026-07-16.
- The related public issue was closed on 2026-07-16.

## Public links

- https://github.com/open-multi-agent/open-multi-agent/issues/204
- https://github.com/open-multi-agent/open-multi-agent/pull/378
- https://github.com/open-multi-agent/open-multi-agent/pull/378#event-28054635710
- https://github.com/open-multi-agent/open-multi-agent/issues/204#issuecomment-4989784266

## Changed public areas

- External-agent documentation.
- Core agent backend configuration types.
- Process backend implementation and I/O handling.
- Shared process-tree cleanup utilities.
- Public package exports and package entrypoint checks.
- Integration example for the process backend.
- Focused tests for process backend behavior, lifecycle cleanup, and backend
  configuration typing.

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This feature record publishes public links, feature scope, validation summaries,
review outcome, and status only.
