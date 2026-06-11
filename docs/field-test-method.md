# Field Test Method

Scarab field tests examine real public software issues without treating public
issue text as proof by itself.

## Method

1. Select a public issue and confirm current public status.
2. Review the repository evidence with SDS.
3. Record the specific finding.
4. Stop if the evidence does not support a clear finding.
5. If a repair is pursued, keep the patch narrow and human reviewed.
6. Validate with project commands where practical.
7. Publish only the case record and public upstream links.

## Modes

- `diagnostic-proof`: SDS recorded a diagnostic finding, but no
  public patch is claimed.
- `repair`: a local or prepared repair exists.
- `diagnostic-proof-and-repair`: the case has both diagnostic proof and repair
  records.
- `upstream-pr-recorded`: a human-reviewed upstream PR or draft PR is publicly
  linked.
- `upstream-accepted`: an upstream maintainer accepted or merged the public PR.
