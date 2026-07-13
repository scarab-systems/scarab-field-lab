---
title: "Directus #24029"
slug: directus-directus-24029
repository: directus/directus
issue_url: https://github.com/directus/directus/issues/24029
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-07-13
---
# Directus #24029

## Public case summary

- Repository: `directus/directus`
- Issue: https://github.com/directus/directus/issues/24029
- Pull request: https://github.com/directus/directus/pull/27899
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reports that a Directus user relationship field backed by
  `$CURRENT_USER.builder_tenant_id` can remain stale after the signed-in user
  saves their own user record.
- The issue is publicly labeled `Bug`, `Help Wanted`, `Studio`, `Med Impact`,
  and `Low Reach`.
- The repair area centered on the current-user save flow in the app.
- Directus already rehydrated the user store after saving the signed-in user's
  record, but permission state was not rehydrated afterward.
- That allowed permission presets using updated `$CURRENT_USER.*` values to keep
  stale current-user context until a later login/session refresh.

## Repair scope

- Rehydrate the permissions store after the current user is saved and the user
  store is refreshed.
- Add a regression test for the current-user save path.
- Add a patch changeset for `@directus/app`.
- Not claimed: This does not change generic relationship dropdown behavior.
- Not claimed: This does not redesign Directus permission presets.
- Not claimed: directus/directus#27899 has not merged at recording.

## Validation record

- Public contribution branch was based on Directus's public `main` branch.
- Latest public pull request head recorded here:
  `5d3d33444a0c0eb2f76752f59a7553bb676826e9`.
- Targeted app tests passed:
  `volta run --node 22.22.2 pnpm --filter @directus/app test app/src/modules/users/routes/item.test.ts app/src/stores/permissions.test.ts app/src/stores/user.test.ts`.
- Targeted app test result: 36 passed, 0 failed.
- Touched-file lint passed:
  `volta run --node 22.22.2 pnpm exec eslint app/src/modules/users/routes/item.vue app/src/modules/users/routes/item.test.ts`.
- Touched-file format check passed:
  `volta run --node 22.22.2 pnpm exec prettier --check --ignore-unknown app/src/modules/users/routes/item.vue app/src/modules/users/routes/item.test.ts .changeset/fresh-users-remember.md`.
- Touched-file style check passed:
  `volta run --node 22.22.2 pnpm exec stylelint app/src/modules/users/routes/item.vue app/src/modules/users/routes/item.test.ts .changeset/fresh-users-remember.md --allow-empty-input`.
- Changeset status check passed:
  `volta run --node 22.22.2 pnpm exec changeset status --since origin/main`.
- Public upstream checks passing at recording: Lint, Format, Stylelint, Unit
  Tests, Changeset Check, and Codecov patch checks.
- Public upstream blockers visible at recording: CLA Assistant required
  contributor action, and the Codecov project check was failing while patch
  coverage checks passed.
- Not claimed: This record does not claim upstream review, CI completion, or
  merge.

## Public review status

- directus/directus#27899 is open against `directus/directus:main`.
- The pull request was opened from the public `scarab-systems/directus` fork.
- The pull request is linked as fixing directus/directus#24029.
- GitHub's mergeability field showed mergeable at recording.
- Contributor CLA action was still required at recording.

## Public links

- https://github.com/directus/directus/issues/24029
- https://github.com/directus/directus/pull/27899

## Changed public files

- .changeset/fresh-users-remember.md
- app/src/modules/users/routes/item.test.ts
- app/src/modules/users/routes/item.vue

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
