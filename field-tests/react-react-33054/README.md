---
title: "React #33054"
slug: react-react-33054
repository: react/react
issue_url: https://github.com/react/react/issues/33054
mode: repair
status: upstream-pr-recorded
recorded_at: 2026-06-12
---
# React #33054

## Public case summary

- Repository: `react/react`
- Issue: https://github.com/react/react/issues/33054
- Pull request: https://github.com/react/react/pull/36709
- Follow-up pull request: https://github.com/react/react/pull/36735
- Mode: repair
- Status: upstream-pr-recorded

## Diagnostic finding

- The public issue reported that React Compiler could generate render-time property reads from a TypeScript non-null assertion inside a callback, causing compiled output to throw when the asserted value was null during render.
- Maintainer discussion identified non-null assertions as values that should contribute nullability information while compiler dependencies are computed.
- The original repair lowered TypeScript non-null assertions into a distinct HIR instruction instead of treating them as a transparent unwrap, so dependency collection could stop at the asserted value.

## Repair scope

- Add `NonNullExpression` HIR handling for the TypeScript compiler path.
- Preserve non-null assertions through code generation.
- Treat non-null expressions as assign-through values for mutation, aliasing, type, scope, pruning, and operand traversal passes.
- Add regression fixtures for callback/event-handler non-null assertion behavior.
- A follow-up React pull request builds on this repair and extends it through additional TypeScript sites plus the Rust compiler mirror.
- Not claimed: react/react#36709 has not merged at recording.
- Not claimed: react/react#36735 has not merged at recording.
- Not claimed: This record does not claim a broad React Compiler dependency redesign beyond the non-null assertion repair path.

## Validation record

- Prettier check: `yarn prettier-check`
- Lint check: `yarn linc`
- Flow check: `yarn flow dom-node`
- Compiler snapshot build: `cd compiler && yarn snap:build`
- Targeted snapshots: `cd compiler && yarn snap -p non-null-assertion-event-handler`
- Targeted snapshots: `cd compiler && yarn snap -p non-null-expression`
- Targeted snapshots: `cd compiler && yarn snap -p non-null-assertion`
- Public PR checks on react/react#36709 at recording: CLA check, access checks, and CodeSandbox package build were passing.
- Public PR checks on react/react#36735 at recording: most TypeScript/compiler checks were passing, with the Rust compiler `Tests` job failing on HIR fixture mismatches.

## Public review status

- react/react#36709 is open, mergeable, and waiting for review at recording.
- react/react#36735 publicly states that it builds on react/react#36709 by Scarab Systems, extends missing TypeScript sites, and adds the Rust compiler mirror.
- react/react#36735 includes public co-author credit for Scarab Systems in its implementation commit.

## Public links

- https://github.com/react/react/issues/33054
- https://github.com/react/react/pull/36709
- https://github.com/react/react/pull/36735
- https://github.com/react/react/issues/34752
- https://github.com/react/react/issues/34194

## Changed public files

- compiler/packages/babel-plugin-react-compiler/src/HIR/BuildHIR.ts
- compiler/packages/babel-plugin-react-compiler/src/HIR/HIR.ts
- compiler/packages/babel-plugin-react-compiler/src/HIR/PrintHIR.ts
- compiler/packages/babel-plugin-react-compiler/src/HIR/visitors.ts
- compiler/packages/babel-plugin-react-compiler/src/Inference/InferMutationAliasingEffects.ts
- compiler/packages/babel-plugin-react-compiler/src/Optimization/DeadCodeElimination.ts
- compiler/packages/babel-plugin-react-compiler/src/Optimization/OutlineJsx.ts
- compiler/packages/babel-plugin-react-compiler/src/ReactiveScopes/CodegenReactiveFunction.ts
- compiler/packages/babel-plugin-react-compiler/src/ReactiveScopes/InferReactiveScopeVariables.ts
- compiler/packages/babel-plugin-react-compiler/src/ReactiveScopes/PruneNonEscapingScopes.ts
- compiler/packages/babel-plugin-react-compiler/src/TypeInference/InferTypes.ts
- compiler/packages/babel-plugin-react-compiler/src/__tests__/fixtures/compiler/non-null-assertion-event-handler.expect.md
- compiler/packages/babel-plugin-react-compiler/src/__tests__/fixtures/compiler/non-null-assertion-event-handler.tsx
- compiler/packages/babel-plugin-react-compiler/src/__tests__/fixtures/compiler/non-null-assertion.expect.md
- compiler/packages/babel-plugin-react-compiler/src/__tests__/fixtures/compiler/non-null-expression.expect.md
- compiler/packages/babel-plugin-react-compiler/src/__tests__/fixtures/compiler/non-null-expression.tsx

## Record limits

This case publishes public links, findings, validation summaries, and status only.
