---
title: "Twenty #22934"
slug: twentyhq-twenty-22934
repository: twentyhq/twenty
issue_url: https://github.com/twentyhq/twenty/issues/22934
mode: diagnostic-proof-and-repair
status: upstream-accepted
recorded_at: 2026-07-23
---
# Twenty #22934

## Public case summary

- Repository: `twentyhq/twenty`
- Issue: https://github.com/twentyhq/twenty/issues/22934
- Pull request: https://github.com/twentyhq/twenty/pull/23008
- Mode: diagnostic-proof-and-repair
- Status: upstream-accepted
- Upstream status: twentyhq/twenty#23008 merged on 2026-07-23.

## Diagnostic finding

- The public issue reported that setting an aggregate operation from a dashboard
  record-table widget footer could throw `ViewField not found` before any
  persistence request was made.
- The visible error occurred in the aggregate footer path, but the repair was
  wider than a local guard around that throw.
- Dashboard record-table widgets use draft view state while page layout editing
  is active. Aggregate changes needed to update that widget draft state and then
  be included in the widget save flow.
- The useful repair boundary was the widget view-field aggregate contract across
  the record-table footer, widget draft state, widget save input, and server-side
  widget view-field upsert logic.

## Repair scope

- Route dashboard record-table widget aggregate changes through the widget field
  update path while page layout editing is active.
- Include `aggregateOperation` when saving record-table widget view fields
  through `upsertViewWidget`.
- Persist aggregate operations in widget view-field create, update, and clear
  flows on the server side.
- Include aggregate state in widget view-load content signatures so draft and
  cancel/reload behavior reflects the current widget configuration.
- Add frontend regression coverage for widget aggregate draft-state behavior.
- Add backend integration coverage for widget aggregate create, update, and
  clear behavior.

## Validation record

- Public contribution branch:
  `scarab-systems/twenty:fix-record-table-widget-aggregate`.
- Latest public pull request head before merge:
  `bb53f063c79e04add5d90fe0e7af9a4ca4aae04a`.
- Public pull request checks before merge included passing front, server, SDK,
  UI, Docker, app, Zapier, Socket, visual-regression, automated-review, and
  blocked-contributor status checks.
- The public pull request recorded local validation for front/server lint,
  typecheck, test, build, targeted widget integration coverage, and whitespace
  hygiene.
- Public pull request status update: merged on 2026-07-23.
- Merge commit: twentyhq/twenty@d1c70ab0bf64312b105a437c522c93bc67327f8f

## Public review status

- The pull request was opened from the public `scarab-systems/twenty` fork.
- The pull request linked and closed twentyhq/twenty#22934.
- The pull request received maintainer approval from `abdulrahmancodes`.
- Twenty maintainer `thomtrp` merged the pull request into
  `twentyhq/twenty:main` on 2026-07-23.
- The issue was closed as completed on 2026-07-23 after the pull request merged.

## Public links

- https://github.com/twentyhq/twenty/issues/22934
- https://github.com/twentyhq/twenty/pull/23008
- https://github.com/twentyhq/twenty/commit/d1c70ab0bf64312b105a437c522c93bc67327f8f

## Changed public files

- packages/twenty-client-sdk/src/metadata/generated/schema.graphql
- packages/twenty-client-sdk/src/metadata/generated/schema.ts
- packages/twenty-client-sdk/src/metadata/generated/types.ts
- packages/twenty-front/src/generated-metadata/graphql.ts
- packages/twenty-front/src/modules/object-record/record-table-widget/components/RecordTableWidgetProvider.tsx
- packages/twenty-front/src/modules/object-record/record-table-widget/contexts/RecordTableWidgetContext.ts
- packages/twenty-front/src/modules/object-record/record-table-widget/utils/__tests__/computeRecordTableWidgetViewLoadContentSignature.test.ts
- packages/twenty-front/src/modules/object-record/record-table-widget/utils/computeRecordTableWidgetViewLoadContentSignature.ts
- packages/twenty-front/src/modules/object-record/record-table/record-table-footer/hooks/useViewFieldAggregateOperation.tsx
- packages/twenty-front/src/modules/page-layout/hooks/useSaveRecordTableWidgetViews.ts
- packages/twenty-front/src/modules/page-layout/widgets/record-table/hooks/useRecordTableWidgetFieldCallbacks.ts
- packages/twenty-front/src/modules/page-layout/widgets/record-table/hooks/useRecordTableWidgetFieldUpdate.ts
- packages/twenty-server/src/engine/metadata-modules/view/dtos/inputs/upsert-view-widget-view-field.input.ts
- packages/twenty-server/src/engine/metadata-modules/view/services/view-widget-upsert.service.ts
- packages/twenty-server/test/integration/metadata/suites/view/upsert-view-widget.integration-spec.ts

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
