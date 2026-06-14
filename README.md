# Scarab Field Lab

<p align="center">
  <img src="assets/scarab-mascot.png" alt="Scarab Systems mascot holding a circuit-board lollipop" width="220">
</p>

Scarab Field Lab is the public case library for selected Scarab Diagnostic Suite field tests.

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

## What This Repo Does Not Show

- SDS source code, product internals, or internal diagnostic rules.
- Private run artifacts, implementation details, or non-public records.
- Secrets or private correspondence.
- Cloned target repositories or vendored upstream source trees.
- Claims that SDS repaired a project by itself.

## Suggest A Field Lab Candidate

If you know of a public open-source issue that looks like cross-layer drift, unclear ownership, software drift, AI-assisted code drift, phase confusion, or a boundary failure. Useful suggestions include the public issue
link, the suspected boundary, reproduction notes if available, and why the case
may be diagnostically interesting. Candidate suggestions are for Field Lab
review only; they are not requests for SDS access or product changes.

## Start Here

- [Scarab Boundary Contract](SCARAB_BOUNDARY_CONTRACT.md)
- [Field Test Method](docs/field-test-method.md)
- [AI-Assisted Public Work Policy](docs/ai-assisted-public-work-policy.md)
- [Public Evidence Policy](docs/public-evidence-policy.md)
- [Contribution Status](CONTRIBUTING.md)

## Mascot

This is Scarab... That is all.
