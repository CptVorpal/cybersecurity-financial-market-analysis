# cyber-industry-viz

A Claude skill that builds or refreshes a five-tab HTML dashboard of the cybersecurity industry landscape. Every figure must trace to a source found during the run; unsourced figures are removed or marked "n/a".

## Install
Open the `.skill` file in Claude and choose Save skill. Or copy this folder into your skills directory.

## Use
Ask: "Build/refresh the cybersecurity industry visualization." Optionally place a prior output at `template/cybersecurity_industry_2026.html` so runs edit data in place.

## Caveats
- Output is only as good as the sources the run finds. Verify figures against the cited primary source before quoting them in coursework or publications.
- Market caps, funding rounds and M&A status change quickly; check dates in the page footer.
- Requires web search and a shell (for `node --check`).
