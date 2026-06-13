---
title: "OpenAPI Generator #23550"
slug: openapitools-openapi-generator-23550
repository: OpenAPITools/openapi-generator
issue_url: https://github.com/OpenAPITools/openapi-generator/issues/23550
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-06-13
---
# OpenAPI Generator #23550

## Public case summary

- Repository: `OpenAPITools/openapi-generator`
- Issue: https://github.com/OpenAPITools/openapi-generator/issues/23550
- Pull request: https://github.com/OpenAPITools/openapi-generator/pull/24022
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported that the Kotlin generator could emit an uncompilable enum for an OpenAPI 3.1 boolean property with `const: true`.
- The generated enum value type was `kotlin.Boolean`, but the enum entry value could be rendered as the string literal `"true"`.
- The repair stayed in Kotlin enum value rendering, where primitive enum literals are converted for generated source output.

## Repair scope

- Keep Kotlin boolean enum values as boolean literals instead of quoted strings.
- Add a focused Kotlin enum value assertion for `kotlin.Boolean`.
- Add a generated-output regression fixture for the reported boolean `const: true` case.
- Not claimed: This does not change Kotlin enum naming behavior.
- Not claimed: This does not redesign OpenAPI 3.1 `const` handling outside the boolean enum literal path.
- Not claimed: OpenAPITools/openapi-generator#24022 has not merged at recording.

## Validation record

- Targeted regression test: passed.
- Build check: `./mvnw clean package`
- Build result: passed; full reactor success.
- Sample regeneration: `./bin/generate-samples.sh ./bin/configs/*.yaml`
- Sample regeneration result: passed; 769 generators.
- Generator docs export: `./bin/utils/export_docs_generators.sh`
- Generator docs export result: passed.
- Diff check: `git diff --check`
- Diff check result: passed.
- Public PR status at recording: open, mergeable, with review/check activity in progress.

## Public links

- https://github.com/OpenAPITools/openapi-generator/issues/23550
- https://github.com/OpenAPITools/openapi-generator/pull/24022
- https://github.com/OpenAPITools/openapi-generator/issues/23550#issuecomment-4689038954

## Changed public files

- modules/openapi-generator/src/main/java/org/openapitools/codegen/languages/AbstractKotlinCodegen.java
- modules/openapi-generator/src/test/java/org/openapitools/codegen/kotlin/AbstractKotlinCodegenTest.java
- modules/openapi-generator/src/test/java/org/openapitools/codegen/kotlin/KotlinClientCodegenModelTest.java
- modules/openapi-generator/src/test/resources/3_1/kotlin/issue23550-boolean-const.yaml

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
