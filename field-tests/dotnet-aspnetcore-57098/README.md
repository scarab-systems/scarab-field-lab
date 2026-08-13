---
title: "ASP.NET Core #57098"
slug: dotnet-aspnetcore-57098
repository: dotnet/aspnetcore
issue_url: https://github.com/dotnet/aspnetcore/issues/57098
mode: diagnostic-proof-and-repair
status: upstream-pr-recorded
recorded_at: 2026-08-01
---
# ASP.NET Core #57098

## Public case summary

- Repository: `dotnet/aspnetcore`
- Issue: https://github.com/dotnet/aspnetcore/issues/57098
- Pull request: https://github.com/dotnet/aspnetcore/pull/68146
- Mode: diagnostic-proof-and-repair
- Status: upstream-pr-recorded
- Upstream status: open pull request recorded on 2026-08-01.

## Diagnostic finding

- The public issue reports a process crash when a connection is aborted and
  disposed close together.
- `DefaultConnectionContext.Abort()` queues cancellation of the
  connection-closed token on the ThreadPool.
- If `DisposeAsync()` disposes the token source before the queued work runs,
  the queued `Cancel()` can observe a disposed `CancellationTokenSource` and
  raise `ObjectDisposedException` on a background worker.
- The useful repair boundary was the queued connection-close cancellation path,
  with process-isolated regression coverage for the background-thread crash
  shape.

## Repair scope

- Make the queued connection-closed cancellation tolerate the already-disposed
  state reached when `DisposeAsync()` wins the race.
- Add a `RemoteExecutor` regression test that forces the queued cancellation to
  run after disposal in a child process.
- Keep the external abort and disposal lifecycle unchanged.
- Not claimed: This does not add a broader synchronization contract between
  abort and disposal.
- Not claimed: This does not change Kestrel transport APIs.

## Validation record

- Public contribution branch was based on ASP.NET Core's public `main` branch.
- Latest public pull request head recorded here:
  `d41dfa309d7b0b00d0cb51119a5f1b1943358d17`.
- Public pull request validation recorded:
  `./restore.sh --no-build-nodejs --no-build-java --no-build-native`.
- Public pull request validation recorded:
  `source ./activate.sh >/dev/null && dotnet test src/Servers/Kestrel/Core/test/Microsoft.AspNetCore.Server.Kestrel.Core.Tests.csproj --no-restore --filter DefaultConnectionContextDisposeAsyncAfterAbortDoesNotCrashProcess`.
- Public pull request validation recorded:
  `source ./activate.sh >/dev/null && dotnet test src/Servers/Kestrel/Core/test/Microsoft.AspNetCore.Server.Kestrel.Core.Tests.csproj --no-restore`.

## Public review status

- dotnet/aspnetcore#68146 is open against `dotnet/aspnetcore:main`.
- The pull request was opened from the public
  `scarab-systems/aspnetcore` fork.
- The pull request fixes dotnet/aspnetcore#57098.
- Maintainer edits are enabled.
- dotnet/aspnetcore#57098 remains open at recording.

## Public links

- https://github.com/dotnet/aspnetcore/issues/57098
- https://github.com/dotnet/aspnetcore/pull/68146

## Changed public files

- src/Servers/Connections.Abstractions/src/DefaultConnectionContext.cs
- src/Servers/Kestrel/Core/test/ConnectionContextTests.cs

## Assistance disclosure

The public pull request includes an AI-assisted coding disclosure. Public
submission and review participation remain human reviewed.

## Record limits

This case publishes public links, findings, validation summaries, and status
only.
