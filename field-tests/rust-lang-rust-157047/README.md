---
title: "Rust #157047"
slug: rust-lang-rust-157047
repository: rust-lang/rust
issue_url: https://github.com/rust-lang/rust/issues/157047
mode: diagnostic-proof-and-repair
status: repair-recorded
recorded_at: 2026-06-11
last_updated: 2026-06-25
---
# Rust #157047

## Public case summary

- Repository: `rust-lang/rust`
- Issue: https://github.com/rust-lang/rust/issues/157047
- Mode: diagnostic-proof-and-repair
- Status: repair-recorded
- Public status: no upstream Rust PR is listed for this case.

## Diagnostic finding

- SDS recorded evidence around Rust compiler type-layout authority while reviewing a stable-to-stable compiler regression involving infinitely recursive structs compiling in optimized mode.
- The relevant boundary was that raw pointer pointee metadata can remain conservative, but codegen still needs to check that computing the pointee layout would succeed.
- The bounded repair stayed in the compiler layout/codegen path and added a focused UI regression test for the recursive raw-pointer pointee case.

## Repair scope

- Force the raw-pointer pointee layout query during codegen when pointee information is requested.
- Preserve conservative raw-pointer metadata behavior: raw pointers do not imply dereferenceability, size, or alignment of the pointee.
- Add a UI test proving the infinitely recursive pointee layout is rejected in the optimized/codegen path.
- Not claimed: This does not change raw pointer dereferenceability semantics.
- Not claimed: This does not broaden general type-layout rules outside the recursive pointee-layout check.
- Not claimed: This does not claim performance neutrality beyond the public Rust try/perf results.

## Validation record

- A focused UI regression was prepared for the recursive raw-pointer pointee
  layout path.
- A local repair was recorded, but no accepted upstream Rust change is claimed.

## Public links

- https://github.com/rust-lang/rust/issues/157047

## Changed public files

- compiler/rustc_middle/src/ty/layout.rs
- tests/ui/codegen/normalization-overflow/raw-ptr-recursive-layout-issue-157047.rs
- tests/ui/codegen/normalization-overflow/raw-ptr-recursive-layout-issue-157047.stderr

## Assistance disclosure

AI assistance, if used, did not determine the diagnostic finding. Public submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status only.
