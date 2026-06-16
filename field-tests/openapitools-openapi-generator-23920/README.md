---
title: "OpenAPI Generator #23920"
slug: openapitools-openapi-generator-23920
repository: OpenAPITools/openapi-generator
issue_url: https://github.com/OpenAPITools/openapi-generator/issues/23920
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-16
---
# OpenAPI Generator #23920

## Public case summary

- Repository: `OpenAPITools/openapi-generator`
- Issue: https://github.com/OpenAPITools/openapi-generator/issues/23920
- Pull request: https://github.com/OpenAPITools/openapi-generator/pull/24023
- PR update: https://github.com/OpenAPITools/openapi-generator/pull/24023#issuecomment-4715563936
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported that `rust-server` client-only builds could miss
  dependencies required by generated model pattern validation.
- Generated shared model code can use `lazy_static` and `regex` when string
  pattern validation is present.
- A client-only feature build needs those dependencies when that generated model
  code is included.

## Repair scope

- Update the generated `rust-server` client feature so pattern validation
  dependencies are available to client-only builds.
- Keep the repair scoped to Rust server output and generated sample manifests.
- Refresh the PR after upstream Rust sample feedback exposed an additional
  sample dependency compatibility issue.
- Not claimed: This does not redesign Rust model validation.
- Not claimed: This does not change non-Rust generators.
- Not claimed: OpenAPITools/openapi-generator#24023 has not merged at recording.

## Validation record

- Targeted Rust server generator regression test: passed.
- Local Rust server sample builds: passed for all-features, no-default-features,
  and client-only sample package coverage.
- Public PR status at update: open, with refreshed upstream CI still running.
- Not claimed: This record does not claim all upstream CI checks have completed.

## Public links

- https://github.com/OpenAPITools/openapi-generator/issues/23920
- https://github.com/OpenAPITools/openapi-generator/pull/24023
- https://github.com/OpenAPITools/openapi-generator/pull/24023#issuecomment-4715563936

## Changed public areas

- Rust server Cargo feature generation.
- Rust server pattern-validation regression coverage.
- Generated Rust server sample manifests.

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
