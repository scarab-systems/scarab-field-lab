---
title: "OpenAPI Generator #23920"
slug: openapitools-openapi-generator-23920
repository: OpenAPITools/openapi-generator
issue_url: https://github.com/OpenAPITools/openapi-generator/issues/23920
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-13
---
# OpenAPI Generator #23920

## Public case summary

- Repository: `OpenAPITools/openapi-generator`
- Issue: https://github.com/OpenAPITools/openapi-generator/issues/23920
- Pull request: https://github.com/OpenAPITools/openapi-generator/pull/24023
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported that the `rust-server` generator could emit model validation code using `lazy_static` and `regex` while a client-only feature build did not include those dependencies.
- Pattern validation in shared generated models can require `lazy_static::lazy_static!` and `regex::Regex` outside the callback path.
- The repair stayed in the `rust-server` Cargo template feature wiring so client-only builds receive the dependencies needed by generated pattern validation code.

## Repair scope

- Add `lazy_static` and `regex` to the generated `client` feature when generated models can emit pattern validation statics.
- Keep callback-only dependencies under the callback branch.
- Add a focused regression spec and test for generated `rust-server` pattern validation models.
- Regenerate the affected current `rust-server` sample Cargo manifests.
- Not claimed: This does not redesign Rust model validation.
- Not claimed: This does not change non-Rust generators.
- Not claimed: OpenAPITools/openapi-generator#24023 has not merged at recording.

## Validation record

- Targeted regression test: passed.
- Targeted CLI build: passed.
- Targeted rust-server sample regeneration: passed.
- Rust sample check: `cargo check --no-default-features --features client`
- Rust sample check result: passed.
- Build check: `./mvnw clean package`
- Build result: passed; full reactor success.
- Sample regeneration: `./bin/generate-samples.sh ./bin/configs/*.yaml`
- Sample regeneration result: passed; 769 generators.
- Generator docs export: `./bin/utils/export_docs_generators.sh`
- Generator docs export result: passed.
- Diff check: `git diff --check`
- Diff check result: passed.
- Public PR status at recording: open, ready for review, mergeable, with public checks green.

## Public links

- https://github.com/OpenAPITools/openapi-generator/issues/23920
- https://github.com/OpenAPITools/openapi-generator/pull/24023

## Changed public files

- modules/openapi-generator/src/main/resources/rust-server/Cargo.mustache
- modules/openapi-generator/src/test/java/org/openapitools/codegen/rust/RustServerCodegenTest.java
- modules/openapi-generator/src/test/resources/3_0/rust-server/pattern-validation.yaml
- samples/server/petstore/rust-server/output/no-example-v3/Cargo.toml
- samples/server/petstore/rust-server/output/openapi-v3/Cargo.toml
- samples/server/petstore/rust-server/output/ops-v3/Cargo.toml
- samples/server/petstore/rust-server/output/ping-bearer-auth/Cargo.toml
- samples/server/petstore/rust-server/output/rust-server-test/Cargo.toml

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
