# Scarab Field Lab

<p align="center">
  <img src="assets/scarab-mascot.png" alt="Scarab Systems mascot holding a circuit-board lollipop" width="220">
</p>

Scarab Field Lab is the public case library for selected Scarab Diagnostic
Suite field tests and public feature pull request records.

Current verified record: **16 merged public open-source contributions across
11 repositories**. The accepted contribution set spans developer tooling,
container tooling, desktop infrastructure, web frameworks, application
platforms, AI infrastructure, and agent systems.

Scarab Diagnostic Suite is proprietary and is not currently distributed as a public installable tool. Public materials describe selected diagnostic field tests and software-drift concepts only.

Scarab does not automate repairs or replace maintainers. It identifies evidence-backed diagnostic findings: boundary failures, repo-truth drift, verification gaps, and repair lanes.

Any repair is performed by maintainers, developers, or authorized agents outside the public Field Lab.

This repository publishes public case records only: public issue and pull request links, specific diagnostic findings, validation notes, claim boundaries, and, when applicable, the public status of a human-reviewed patch or upstream pull request. It does not contain SDS source code, internal diagnostic rules, product internals, private run artifacts, or implementation details.

Scarab Diagnostic Suite is a mechanical diagnostic layer. It inspects repository evidence, compares expected and observed behavior, and records specific findings. It is not an AI coding agent, does not use AI to determine diagnostic truth, and does not submit unattended patches. AI assistance may help after bounded diagnostic evidence exists with patch drafting or summaries, but public submissions remain human reviewed.

## What This Repo Shows

- Public issue and pull request links.
- Sanitized diagnostic evidence.
- Whether a case stayed diagnostic-only, became a local repair candidate, or
  became an upstream PR.
- Public validation summaries.
- Assistance notes where relevant.
- The current field-test index: [FIELD_TEST_INDEX.md](FIELD_TEST_INDEX.md).
- The current feature PR index: [FEATURE_PR_INDEX.md](FEATURE_PR_INDEX.md).

## Merged Contribution Summary

This summary is derived from public Field Lab records and live merged pull
request status checked on 2026-07-25.

| Repository | Merged contributions | Area | Merged PRs |
| --- | ---: | --- | --- |
| `directus/directus` | 1 | Application platform | [#27899](https://github.com/directus/directus/pull/27899) |
| `docker/compose` | 1 | Container tooling | [#13831](https://github.com/docker/compose/pull/13831) |
| `electron/electron` | 1 | Desktop platform | [#52238](https://github.com/electron/electron/pull/52238) |
| `emersion/xdg-desktop-portal-wlr` | 1 | Linux desktop infrastructure | [#393](https://github.com/emersion/xdg-desktop-portal-wlr/pull/393) |
| `microsoft/agent-framework` | 1 | Agent framework | [#7189](https://github.com/microsoft/agent-framework/pull/7189) |
| `NVIDIA/NemoClaw` | 2 | AI infrastructure | [#7254](https://github.com/NVIDIA/NemoClaw/pull/7254), [#7406](https://github.com/NVIDIA/NemoClaw/pull/7406) |
| `open-multi-agent/open-multi-agent` | 2 | Multi-agent systems | [#377](https://github.com/open-multi-agent/open-multi-agent/pull/377), [#378](https://github.com/open-multi-agent/open-multi-agent/pull/378) |
| `OpenAPITools/openapi-generator` | 1 | API developer tooling | [#24022](https://github.com/OpenAPITools/openapi-generator/pull/24022) |
| `pnpm/pnpm` | 3 | Package management | [#12301](https://github.com/pnpm/pnpm/pull/12301), [#12327](https://github.com/pnpm/pnpm/pull/12327), [#12841](https://github.com/pnpm/pnpm/pull/12841) |
| `sveltejs/kit` | 2 | Web framework | [#16268](https://github.com/sveltejs/kit/pull/16268), [#16423](https://github.com/sveltejs/kit/pull/16423) |
| `twentyhq/twenty` | 1 | Open-source application platform | [#23008](https://github.com/twentyhq/twenty/pull/23008) |

Repeat contributions have been accepted in `pnpm/pnpm`, `NVIDIA/NemoClaw`,
`open-multi-agent/open-multi-agent`, and `sveltejs/kit`. Those records show
work continuing past a first accepted patch: learning local conventions,
responding to review, adding repository-native tests, and keeping the final
claim tied to the target project's own evidence.

NVIDIA's NemoClaw v0.0.96 release announcement publicly thanked
`@scarab-systems` for the accepted NemoClaw contributions in
[NVIDIA/NemoClaw#7254](https://github.com/NVIDIA/NemoClaw/pull/7254) and
[NVIDIA/NemoClaw#7406](https://github.com/NVIDIA/NemoClaw/pull/7406).

## Contribution Method

Public Field Lab records document a repeatable contribution method without
publishing private diagnostic internals:

1. Select a real public issue, failure report, or repository drift surface.
2. Reproduce, confirm, or narrow the observed behavior when possible.
3. Trace the relevant code path and repository ownership boundary.
4. Gather public repository evidence and current project conventions.
5. Develop a minimal, scoped patch when the evidence supports one.
6. Add or update tests for the behavior being changed.
7. Run repository-native checks and record what passed or remained unverified.
8. Review the final diff for scope, claim limits, and public evidence.
9. Respond to maintainer review.
10. Record the public outcome after review or merge.

Proprietary diagnostic tooling may support repository analysis, evidence
gathering, patch validation, and scope control. Public submissions are still
checked against the target repository's code, conventions, tests, maintainer
requirements, and final review.

## Selected Case Records

The detailed records remain in the case files. These selected entries show the
range of accepted work without replacing the underlying evidence.

| Record | Merged PR | Focus | Public validation or review signal |
| --- | --- | --- | --- |
| [pnpm #12240](field-tests/pnpm-pnpm-12240/README.md) | [pnpm/pnpm#12301](https://github.com/pnpm/pnpm/pull/12301) | `self-upgrade` dependency-status handling when no manifest is present. | Targeted Jest tests, TypeScript build, and package lint passed before merge. |
| [NemoClaw #6042](field-tests/nvidia-nemoclaw-6042/README.md) | [NVIDIA/NemoClaw#7254](https://github.com/NVIDIA/NemoClaw/pull/7254) | Empty policy-preset resume contract coverage. | Targeted Vitest and integration tests passed; maintainer approved and merged the test-only contribution. The v0.0.96 release announcement later credited the contribution. |
| [SvelteKit #9785](field-tests/sveltejs-kit-9785/README.md) | [sveltejs/kit#16268](https://github.com/sveltejs/kit/pull/16268) | Streamed server-data rejection race handling. | Repository checks and focused Playwright regressions passed; maintainer review retargeted and approved the PR. |
| [Docker Compose #13613](field-tests/docker-compose-13613/README.md) | [docker/compose#13831](https://github.com/docker/compose/pull/13831) | Variable extraction over unresolved Compose config values. | Focused and package Go tests, lint, and diff checks passed before merge. |
| [Microsoft Agent Framework #7160](field-tests/microsoft-agent-framework-7160/README.md) | [microsoft/agent-framework#7189](https://github.com/microsoft/agent-framework/pull/7189) | Python MCP sampling conversion for structured tool-use results. | Public checks and maintainer approvals completed before merge. |
| [Open Multi Agent #378](feature-prs/open-multi-agent-378/README.md) | [open-multi-agent/open-multi-agent#378](https://github.com/open-multi-agent/open-multi-agent/pull/378) | Generic process backend for external agents. | Maintainer review identified blockers; follow-up revisions landed before merge. |

## What This Repo Does Not Show

- SDS source code, product internals, or internal diagnostic rules.
- Private run artifacts, implementation details, or non-public records.
- Secrets or private correspondence.
- Cloned target repositories or vendored upstream source trees.
- Claims that SDS repaired a project by itself.

## Suggest A Field Lab Candidate

Use the [Ideas discussion form](https://github.com/scarab-systems/scarab-field-lab/discussions/new?category=ideas)
to suggest a public Field Lab candidate.

Useful suggestions include the public issue link, the suspected boundary,
reproduction notes if available, and why the case may be diagnostically
interesting. Candidate suggestions are for Field Lab review only; they are not
requests for SDS access or product changes.

## Start Here

- [Scarab Boundary Contract](SCARAB_BOUNDARY_CONTRACT.md)
- [Field Test Method](docs/field-test-method.md)
- [Feature PR Index](FEATURE_PR_INDEX.md)
- [AI-Assisted Public Work Policy](docs/ai-assisted-public-work-policy.md)
- [Public Evidence Policy](docs/public-evidence-policy.md)
- [Contribution Status](CONTRIBUTING.md)

## Sponsorship

This work is independently researched, implemented, tested, and documented.
Sponsorship supports dedicated time for public open-source diagnostics, scoped
patch development, regression testing, documentation, and contribution
follow-through.

Sponsor-selected work does not guarantee maintainer review, acceptance, merge,
release inclusion, or downstream adoption. Maintainers remain the final
authority for their own projects.

Sponsor Scarab Systems:
[github.com/sponsors/scarab-systems](https://github.com/sponsors/scarab-systems?metadata_campaign=field_lab)

## Mascot

This is Scarab... That is all.
