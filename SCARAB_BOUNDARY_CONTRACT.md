# Scarab Boundary Contract

The Scarab Boundary Contract is the public promise behind this field lab:

**SDS finds evidence. People make claims. Maintainers decide.**

## The Boundary

Scarab Diagnostic Suite is a mechanical diagnostic layer. It records findings
from deterministic checks of repository evidence. It does not use generative
model reasoning to decide what is true.

SDS may:

- inspect repository evidence;
- compare expected and observed behavior;
- record contradictions or missing proof;
- preserve diagnostic findings for human review.

SDS may not:

- invent a diagnosis from model reasoning;
- choose speculative repairs without documented evidence;
- generate or submit unattended upstream patches;
- treat issue text, maintainer comments, or private notes as diagnostic truth.

## AI Assistance Boundary

AI assistance may be used only after diagnostic evidence exists. At that point
it may help prepare a narrow patch, summarize evidence, organize validation
notes, or draft maintainer-facing explanations.

The human operator remains accountable for:

- reviewing the diagnostic evidence;
- deciding whether a patch is appropriate;
- understanding and owning the public claim;
- running practical validation;
- participating in upstream review.

Upstream maintainers remain the final authority over their own project.

## Public Evidence Boundary

This repo publishes case records and public links only. It excludes SDS product
materials, target repository clones, and non-public records.
