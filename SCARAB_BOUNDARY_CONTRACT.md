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

AI assistance may be used only after bounded diagnostic evidence exists. At
that point it may help prepare a narrow patch, summarize evidence, organize
validation notes, or draft maintainer-facing explanations.

The human operator remains accountable for:

- reviewing the diagnostic evidence;
- deciding whether a patch is appropriate;
- understanding and owning the public claim;
- running practical validation;
- participating in upstream review.

Upstream maintainers remain the final authority over their own project.

## Public Evidence Boundary

Scarab Diagnostic Suite is proprietary and not open source. This repository
publishes case records and public links only. Field Lab records do not include
SDS source code, internal diagnostic rules, product internals, private run
artifacts, implementation details, target repository clones, or non-public
records.

Public participation is welcome at the Field Lab layer: suggesting public
open-source issues, providing reproduction context, pointing to public evidence,
or discussing whether a case record is clear. Participation does not imply
access to SDS internals or co-ownership of the diagnostic method.
