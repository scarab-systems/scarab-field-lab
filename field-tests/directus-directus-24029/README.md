---
title: "Directus #24029"
slug: directus-directus-24029
repository: directus/directus
issue_url: https://github.com/directus/directus/issues/24029
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-07-13
---
# Directus #24029

## Public case summary

- Repository: `directus/directus`
- Issue: https://github.com/directus/directus/issues/24029
- Pull request: https://github.com/directus/directus/pull/27899
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: directus/directus#27899 merged on 2026-07-24.

## Diagnostic finding

- The public issue reports that a Directus user relationship field backed by
  `$CURRENT_USER.builder_tenant_id` can remain stale after the signed-in user
  saves their own user record.
- The issue is publicly labeled `Bug`, `Help Wanted`, `Studio`, `Med Impact`,
  and `Low Reach`.
- The repair area centered on the current-user save flow in the app.
- Directus already refreshed the user store after saving the signed-in user's
  record, but related permission preset state could keep the prior
  `$CURRENT_USER.*` values until a later session refresh.
- During review, the maintainers confirmed that `/permissions/me` already
  returns resolved preset values. That meant the current-user save flow needed
  to refresh permissions after the user record refresh, while redundant
  client-side preset parsing could be removed.

## Repair scope

- Refresh permissions after the signed-in user is saved and the current-user
  store has been refreshed.
- Remove redundant client-side preset parsing now that resolved presets are
  supplied by `/permissions/me`.
- Preserve display of a forbidden related item key by matching the Directus
  `FORBIDDEN` error code rather than only relying on status.
- Add focused coverage for the permissions preset behavior and relationship
  field fallback.
- Add patch changesets for `@directus/app`.
- Not claimed: This does not redesign Directus permission presets.
- Not claimed: This does not change the server-side permission resolver
  contract.

## Validation record

- Public contribution branch was based on Directus's public `main` branch.
- Latest public pull request head before merge:
  `6bad91ae45fca40465c2289b1aa1eac5d0eed805`.
- During review, public comments recorded focused local app checks for the
  current-user save path, permissions preset behavior, relationship field
  fallback, touched-file lint/format checks, and whitespace hygiene.
- Public pull request checks visible at merge included passing CLA Assistant,
  Changeset Check, Lint, Format, Stylelint, Unit Tests, Codecov patch checks,
  and Codecov project checks.
- Public pull request status update: merged into `directus/directus:main` on
  2026-07-24.
- Merge commit: directus/directus@a2ad7462e30ac257d65f5514d35121ee0da55028

## Public review status

- directus/directus#27899 was opened against `directus/directus:main`.
- The pull request was opened from the public `scarab-systems/directus` fork.
- The pull request is linked as fixing directus/directus#24029.
- Maintainer `ComfortablyCoding` reproduced the issue after additional
  reproduction details were supplied.
- During review, the maintainers initially explored moving preset resolution to
  consumption sites. After further investigation, `ComfortablyCoding` posted a
  corrective review note confirming that the original refresh-permissions
  approach was the right approach for the current implementation.
- `ComfortablyCoding` approved and merged the pull request on 2026-07-24.
- The merged pull request closed directus/directus#24029 as completed.

## Public links

- https://github.com/directus/directus/issues/24029
- https://github.com/directus/directus/pull/27899
- https://github.com/directus/directus/pull/27899#issuecomment-5070913947
- https://github.com/directus/directus/pull/27899#issuecomment-5072015800
- https://github.com/directus/directus/commit/a2ad7462e30ac257d65f5514d35121ee0da55028

## Changed public files

- .changeset/brave-otters-cheer.md
- .changeset/silent-melons-relate.md
- app/src/composables/use-relation-single.test.ts
- app/src/composables/use-relation-single.ts
- app/src/modules/users/routes/item.vue
- app/src/stores/permissions.test.ts
- app/src/stores/permissions.ts
- app/src/utils/parse-preset.test.ts
- app/src/utils/parse-preset.ts
- contributors.yml

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
