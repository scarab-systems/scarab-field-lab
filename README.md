# Scarab Field Lab

<p align="center">
  <img src="assets/scarab-mascot.png" alt="Scarab Systems mascot holding a circuit-board lollipop" width="220">
</p>

Scarab Field Lab is the public case library for Scarab Diagnostic Suite field
tests.

Each field test records public links, a specific diagnostic finding, validation
notes, and, when applicable, the public status of a human-reviewed patch or
upstream pull request. The repo publishes the case record only.

Scarab Diagnostic Suite is a mechanical diagnostic layer. It inspects repo
evidence, compares expected and observed behavior, and records specific
findings. It is not an AI coding agent, does not generate diagnostic truth
through model reasoning, and does not submit unattended patches. AI assistance
may help later with patch drafting or summaries, but public submissions remain
human reviewed.

## What This Repo Shows

- Public issue and pull request links.
- Sanitized diagnostic evidence.
- Whether a case stayed diagnostic-only, became a local repair candidate, or
  became an upstream PR.
- Public validation summaries.
- Assistance notes where relevant.
- The current field-test index: [FIELD_TEST_INDEX.md](FIELD_TEST_INDEX.md).

## What This Repo Does Not Show

- SDS product materials or non-public records.
- Secrets or private correspondence.
- Cloned target repositories or vendored upstream source trees.
- Claims that SDS repaired a project by itself.

## Start Here

- [Scarab Boundary Contract](SCARAB_BOUNDARY_CONTRACT.md)
- [Field Test Method](docs/field-test-method.md)
- [AI-Assisted Public Work Policy](docs/ai-assisted-public-work-policy.md)
- [Public Evidence Policy](docs/public-evidence-policy.md)
- [Contribution Status](CONTRIBUTING.md)

## Mascot

The Scarab Systems mascot is included as a small trust mark. It may be used in
Scarab-owned public field-test communication when a little warmth is appropriate,
but it should not replace evidence, validation, or maintainer-facing clarity.
